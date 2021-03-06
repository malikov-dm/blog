# Шпаргалка по использованию Git 6

## Объединение (git merge)

### Слияние изменений: git merge

Если необходимо провести слияние двух веток, у которых вершины уже разошлись, то применить *fast-forward* слияние уже не получится.  
В этом случае слияние будет *истинным*. Файлы из веток будут сравниваться с файлом из последнего общего коммита веток.  
В случае, если один и тот же фрагмент файла менялся в обоих ветках, возникнет конфликт, который git, скорее всего, не сможет решить самостоятельно.  
В этом случае, нужно будет дополнительно разбираться с контентом файла.

Для примера, сделаем слияние ветки `feature` с веткой `master`:

```bash
git checkout master                     # перемещаемся на ветку master
git merge feature                       # сливаем ветку feature с master
```

Допустим, в одном из файлов возник конфликт слияния. В этом случае, мы увидим что-то вроде этого:

```bash
<<<<<<< HEAD
foo();
=======
bar();
>>>>>>> feature
```

Слияние прервется и будет необходимо разрешить конфликт слияния.

#### Игнорирование изменений в одной из веток при слиянии

В случае, если мы точно знаем, в какой ветке находится правильная версия файлов, мы можем применять `git checkout` с соответствующим флагом:

```bash
git checkout --ours <file_path1>        # получит версию файла из той ветки, где мы сейчас находимся
git checkout --theirs <file_path1>      # получит версию файла из сливаемой ветки
git checkout --merge <file_path1>       # продолжить слияние с маркерами конфликта
```

#### Откат изменений при неудачном слиянии

```bash
git reset --hard                        # hard reset всегда поможет в трудной ситуации
git reset --merge                       # не трогает непроиндексированные файлы, если вдруг слияние проводилось на ветке с незакоммиченными изменениями
git merge --abort                       # то же самое
```

#### Продвинутый анализ конфликтов

По умолчанию значением ключа `--conflict` является `merge`. Но если заменить его на `diff3`, то в отображение конфликта добавится базовый фрагмент файла:

```bash
git checkout --conflict=diff3 --merge <file_path1>
```

В этом случае, мы увидим что-то вроде этого:

```bash
<<<<<<< ours
foo();
||||||| base
helloWorld();
=======
bar();
>>>>>>> theirs
```

Такой вывод конфликтов можно настроить по умолчанию:

```bash
git config --global merge.conflictstyle diff3
```

#### Принятие коммита

После разрешения конфликта, git все еще не знает, какой файл коммитить, у него все еще есть данные по всем трем версиям:

```bash
git show :1:<file_path1>                # базовый файл
git show :2:<file_path1>                # версия из ветки, куда сливаются изменения (master)
git show :3:<file_path1>                # версия из ветки, из которой сливаются изменения (feature)
```

Поэтому при коммите возникает ошибка. Разрешить ее просто - нужно просто добавить файл с разрешенным конфликтом в индекс `git add <file_path1>`.  
После чего - коммит:

```bash
git commit                              #
git merge --continue                    # та же самая команда commit
```

### Коммит слияния

При истинном слиянии образуется новый коммит, называемый коммитом слияния. Отличается тем, что имеет два родительских коммита. Если посмотреть информацию об этом коммите, будет показано следующее:

```bash
C:\Git\_Local\jsru [master]> git show
commit a7452e923286c7e5e5c07a96a3af0311198a56e6 (HEAD -> master)
Merge: 9fd130b 5efd0da                  # указаны два родительских коммита
...
```

Для коммита слияния можно указать комментарий или записать туда лог изменений:

```bash
git merge feature -m 'Comment'          # указать комментарий
git merge feature --log                 # добавить в развернутый комментарий лог изменений
```

Если из коммита слияния попробовать посмотреть историю ветки, то в ней окажутся и коммиты из смердженных веток, так как формально они теперь тоже принадлежат этой ветке. Чтобы посмотреть коммиты только из этой ветки, для коммита слияния используется флаг `--first-parent`

```bash
git log master --oneline --first-parent
```

### Отмена слияния

Отмена слияния - это просто откат на один шаг назад по ветке master

```bash
git reset --hard "@~"                   # откат на один коммит назад
```

Все коммиты слияния при откате также остаются в репозитории, и к ним можно вернуться (если, конечно, они не были удалены как недостижимые автоматически по прошествии какого-то количества времени)

### Семантические конфликты

При слиянии может возникнуть ситуация, когда git не находит конфликтов в файлах, но проект после слияния перестает работать, так как в разных местах были использованы разные версии метода (например). Чтобы проверить работоспособность проекта перед коммитом, можно сначала сделать слияние без коммита:

```bash
git merge feature --no-commit           # коммит не происходит и у нас есть возможность исправить код
git merge --continue                    # продолжит слияние (создаст коммит слияния)
git commit                              # то же самое
```

### Истинное слияние с сохранением веток, запрет перемоток --no-ff

В этом случае, в отличие от слияния перемоткой, будет создан отдельный коммит слияния.  
Включать и выключать ключ --no-ff по умолчанию для репозитория и веток можно в настройках.

### Слияние без связи с источником: merge --squash

```bash
git merge --squash feature              # ключ --squash указывает на то, что коммит слияния не будет указывать на второго родителя из ветки feature
```

В то же время ветка `feature` как бы пропадает из истории разработки. В `master` будет только последняя версия.

### Слияние без конфликтов через драйвер union, свои драйверы

В `.gitattributes` можно указать специальный драйвер для какого-нибудь файла:

```.gitattributes
/<file_path> merge=union
```

Тогда при возникновении конфликтов будут взяты обе версии фрагмента кода.  
Можно использовать свои скрипты для слияния файлов, они устанавливаются через `config` и подключаются также через `.gitattributes`

### Стратегии слияния

***

[< Просмотр (git diff)](git-cheat-sheet-5.md) |