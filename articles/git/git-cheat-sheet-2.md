# Шпаргалка по использованию Git 2

## Инициализация репозитория, подключение к удаленному репозиторию, история

### Начало работы с файлами и изменениями

```bash
git init                                    # инициализация репозитория в текущей папке
git add .                                   # добавление всех файлов текущего каталога в индекс
git add -p <file_name1>                     # git пробежит по найденным изменениям файла и спросит, добавлять его в индекс или нет
git add <file_name1> <file_name2> <...>     # добавление указанных файлов и каталогов
git commit -m "First Commit"                # создание коммита с комментарием в командной строке
git commit                                  # создание коммита с открытием внешнего текстового редактора
git commit -a                               # создание коммита без необходимости писать git add
git commit -am "Сообщение"                  # коммит без git add и с комметарием прямо в командной строке
git commit -m "Сообщение" <file_name1>      # коммит указанного файла минуя индекс
```

### Права на файл

В Linux можно столкнуться с сохранением прав файла - это тоже модификации. При необходимости - искать по запросам git linux chmod

### Проверка состояния и истории

***Состояние***

```bash
git status
```

***История***

Просмотр коммита:

```bash
## последний коммит
## кратко
git show
## или так
git show --pretty=fuller

## определенный коммит по хэшу
## кратко
git show 75ab54
## или так
git show 75ab54 --pretty=fuller
```

Различные ключи отвечают за информацию, отображаемую в терминале

```bash
git log
git log --pretty=oneline
git log --pretty=oneline --max-count=2
git log --pretty=oneline --since='5 minutes ago'
git log --pretty=oneline --until='5 minutes ago'
git log --pretty=oneline --author=<your name>
git log --pretty=oneline --all
```

Форматирование вывода

```bash
git log --all --pretty=format:"%h %cd %s (%an)" --since='7 days ago'
```

или так

```bash
git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short
```

### Подключение внешнего репозитория и начало работы

Через `git clone`:

```bash
git clone https://malikov-dm@bitbucket.org/malikov-dm/metanit.git
```

Эта команда создаст папку с репозиторием, никакую папку больше создавать не нужно.
То есть, если вы находитесь в папке `Bitbucket`, то выполнять команду `git clone` надо в ней.

Через `git pull`:

```bash
git init
git remote add origin <repository_url>
git pull origin master
```

Инициализируем локальный репозиторий, добавляем ссылку на удаленный репозиторий, вытягиваем себе все изменения

```bash
git add .
git commit -am "initial commit"
git push -u origin master
```

Добавляем содержимое текущей директории в индекс, коммитим, отправляем в удаленный репозиторий.

Ключ `-u` свяжет локальную ветвь `master` и удаленную ветвь `origin`, после чего можно будет пользоваться сокращенной фрормой `git push`.

В строке терминала должен высвечиваться значок `≡`

```bash
[master ≡]
````

[< Конфигурация и подготовка к работе](git-cheat-sheet-1.md) | [Работа с ветками >](git-cheat-sheet-3.md)