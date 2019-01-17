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
git show :1:<file_path1>                # версия из ветки, куда сливаются изменения (master)
git show :1:<file_path1>                # версия из ветки, из которой сливаются изменения (feature)
```

Поэтому при коммите возникает ошибка. Разрешить ее просто - нужно просто добавить файл с разрешенным конфликтом в индекс `git add <file_path1>`.  
После чего - коммит:

```bash
git commit                              #
git merge --continue                    # та же самая команда commit
```

***

[< Просмотр (git diff)](git-cheat-sheet-5.md) |