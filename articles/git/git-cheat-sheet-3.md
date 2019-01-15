# Шпаргалка по использованию Git 3

## Работа с ветками

### Начало работы с ветками

```bash
git branch                              # просмотр информации о ветках
git branch -v                           # просмотр информации о ветках с последним коммитом
```

Указатель HEAD указывает на тот коммит, в котором мы находимся сейчас.

***Создание ветки***

```bash
git branch <branch_name>                # создает новую ветку из того коммита, в котором мы находимся сейчас
```

Например, в моем репозитории после вышеуказанных манипуляций при вызове команды `git hist` такая ситуация:

```bash
* 326b0d9 2019-01-14 | Поправил навигацию по статьям по Git (HEAD -> master, origin/master, origin/HEAD, feature) [Dmitriy Malikov]
```

> `HEAD -> master` - показывает, что указатель HEAD сейчас указывает на ветку master, а ветка мастер заканчивается на текущем коммите  
> `origin/master, origin/HEAD` - показывает, где находятся указатели на ветку и текущий коммит в удаленном репозитории  
> `feature` - показывает, что ветка feature заканчивается на текущем коммите

***Переключение на ветку***

По сути, переключение на ветку или коммит - это изменение ветки или коммита, на который указывает HEAD

```bash
git checkout <branch_name>              # переключение на ветку
git checkout -b <branch_name>           # создать ветку и сразу переключиться на нее
```

***Переключение на другую ветку при внесенных изменениях***

```bash
git checkout -f <branch_name>           # -f(--force) форсированно переключает на другую ветку с потерей изменений
git checkout -f HEAD                    # просто отмена внесенных изменений
git checkout -f                         # если не указывать ветку или коммит, то будет применен HEAD
```

```bash
git stash                               # добавляет изменения в stash и очищает индекс. После этого переключиться на другую ветку можно без проблем
git stash pop                           # вернуть изменения из stash
```

***Создание ветки из нескольких коммитов, передвижение веток***

```bash
git branch <branch_name>                # создание ветки branch_name
git branch <branch_name> <commit_id>    # создание ветки branch_name на коммите commit_id
git branch -f <branch_name> <commit_id> # перемещение ветки branch_name на коммит commit_id
```

Перед перемещением ветки необходимо уйти с нее на другую ветку (например, feature)

```bash
git branch -B <branch_name> <commit_id> # форсированное перемещение ветки на коммит
```

***Detouched HEAD***

```bash
git checkout <commit_id>                # чекаут указанного коммита
```

В таком состоянии HEAD указывает на какой-то коммит, но не на ветку. Коммиты из такого состояния также будут находиться вне какой-то из веток. Поэтому, если мы уйдем из этого коммита, вернуться в него будет довольно сложно. Если это не были коммиты, сделанные по ошибке, то лучше создать для них ветку (иак подсказывает Git при переходе в состояние отделенной HEAD)

```bash
git branch <new-branch-name> <commit_id>
```

Перенести изменения отдельного коммита можно с помощью `git cherry-pick`.

***Переключение версий файла***

Откатить изменения файла на состояние какого-либо коммита можно командой `git checkout`. В этом случае не произойдет переключения ветки, только файл получит состояние на момент указанного коммита.

```bash
git checkout <commit_id> <file_path>
git checkout HEAD <file_path>           # вернет из HEAD файлы в индекс и рабочую директорию
git checkout <file_path>                # вернет файлы из индекс в рабочую директорию
```

***История***

Просмотреть историю можно командой `git log`

```bash
git log
git log --oneline
git log <branch_name> --oneline
git log <commit_id> --oneline
```

Просмотреть информацию о коммите можно командой `git show`

```bash
git show                                # по умолчанию покажет инфо о коммите, на который указывает HEAD
git show <branch_name>                  #
git show <commit_id>                    #
git show <commit_id>~                   # ~ указывает на родителя коммита commit_id или на родителя любого другого указателя (HEAD (== @), <branch_name> и т.д.)
git show <commit_id>~~                  # == git show <commit_id>~2
git show @~                             # == git show HEAD~
git show "@~"                           #
```

Просмотреть содержимое файла на момент какого-то коммита можно также командой `git show`

```bash
git show <commit_id>:<file_path>
```

Можно также пользоваться поиском по файлам, чтобы посмотреть ближайший коммит, в котором этот файл был изменен:

```bash
git show :/"<search_string>"
```

***Объединение веток***

```bash
git checkout master                     # переключаемся на ветку, КУДА будем переносить изменения
git merge feature                       # мерджим ветку, ОТКУДА переносим изменения
```

> По умолчанию, ветки объединяются методом Fast-forward  
> При таком методе указатель ветки КУДА мы сливаем изменения (master),  
> поочередно проходит все коммиты до указателя ветки, ОТКУДА мы сливаем изменения (feature)

Если слияние нам по каким-то причинам не понравилось, можно вернуть указатель master на свое прежнее место. В этом нам поможет указатель `ORIG_HEAD`

```bash
git branch -f master ORIG_HEAD          # ORIG_HEAD указывает на коммит, на который указывал master ДО слияния
```

По сути, Fast-forward можно повторить командой `git checkout`, но только в простейших случаях merge

```bash
git checkout -B master feature          # команда передвинет указатель master на указатель feature
```

***Удаление веток***

```bash
git branch -d <branch_name>             # команда удаляет УКАЗАТЕЛЬ, коммиты остаются на месте. Ветка должна быть объединенной с какой-либо остающейся.
git branch -D <branch_name>             # команда форсированно удаляет УКАЗАТЕЛЬ, коммиты остаются на месте.
```

Если ветку удалить без объединения, коммиты из этой ветки будут через некторое время удалены.  
Восстановить ветку по сути нельзя, но можно создать новую ветку с таким же именем, которая будет указывать на тот же коммит. Разницы никакой.

***Изменение ссылок***

Git записывает все манипуляции со ссылками (коммиты, создание, перенос, чекаут, объединение и т.д.) в *reflog*.  

```bash
git reflog                              # reflog для HEAD
git reflog <link_name>                  # reflog для ссылки (master и пр.)
```

С помощью *reflog* можно отслеживать операции с указателями. Например, для удаленной ветки можно найти идентификатор последнего коммита и восстановить ветку.

```bash
git branch feature HEAD@{4}             # из reflog мы узнали, что под такой ссылкой был последний коммит в ветку feature. Чтобы восстановить ветку feature, создадим новую с тем же названием и указывающую на тот же коммит.
git branch feature "HEAD@{4}"           # для PowerShell двойные кавычки
git branch feature 'HEAD@{4}'           # для PowerShell одинарные кавычки
```

Также ссылки из *reflog* можно использовать и в других командах git:

```bash
git checkout @{-1}                      # переключится на предыдущую ветку
```

В примере выше Git будет искать в *reflog* слово `checkout`, после чего найдет ветку, для которой была сделана эта запись в *reflog* и переключится на нее (*если она еще существует, конечно*).

## Теги

### Начало работы с тегами

Тег - это еще один вид ссылок на коммиты.  
Тэг всегда указывает на один и тот же коммит.  
Такой островок стабильности среди изменчивых ссылок Git.

Просмотреть теги репозитория можно командой `git tag`

```bash
git tag -n                              # список тегов с комментариями коммитов
git tag -n -l '<mask_string>'           # список тегов, подходящих по маске <mask_string>
```

***Создание тега***

```bash
git tag <tag_name> <commit_id>          # ссылка на коммит через commit_id
git tag <tag_name> <branch_name>        # ссылка на коммит через имя ветки

```

Тег с аннотацией содержит информацию о том, кто сделал этот коммит, а также комментарий.

```bash
git tag -a <tag_name> <commit_id>
```

Комментарий к тегу можно задать прямо в командной строке

```bash
git tag -a -m '<cmment_text>' <tag_name> <commit_id>
```

***Экспорт проекта по тегу***

Если вам нужно экспортировать проект на какой-то момент, который отмечен тегом, то нужно воспользоваться командами `git describe` и `git archive`.  
Подробнее - в документации.

[< Инициализация репозитория, подключение к удаленному репозиторию, история](git-cheat-sheet-2.md) | [Отмена изменений (reset) >](git-cheat-sheet-4.md)