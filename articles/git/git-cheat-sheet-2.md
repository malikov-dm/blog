# Шпаргалка по использованию Git 2

## Инициализация репозитория, подключение к удаленному репозиторию, история

### Начало работы с файлами и изменениями

```bash
git init                                    #инициализация репозитория в текущей папке
git add .                                   #добавление всех файлов текущего каталога в индекс
git add <file_name1> <file_name2> <...>     #добавление указанных файлов и каталогов
git commit -m "First Commit"                #создание коммита с комментарием в командной строке
git commit                                  #создание коммита с открытием внешнего текстового редактора
```

### Проверка состояния и истории

***Состояние***

```bash
git status
```

***История***

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