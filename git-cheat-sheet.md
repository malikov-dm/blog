# Шпаргалка по использованию Git

## Настройка конфига и алиасов

***Имя и почта***

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@whatever.com"
```

***Окончание строк***

```bash
git config --global core.autocrlf true
git config --global core.safecrlf true
```

***Отображение юникода***

```bash
git config --global core.quotepath off
```

## Начало работы с файлами и изменениями

```bash
git init                                    #инициализация репозитория в текущей папке
git add .                                   #добавление всех файлов текущего каталога в индекс
git add <file_name1> <file_name2> <...>     #добавление указанных файлов и каталогов
git commit -m "First Commit"                #создание коммита с комментарием в командной строке
git commit                                  #создание коммита с открытием внешнего текстового редактора
```

## Подключение внешнего репозитория и начало работы

```bash
git init
git remote add origin <repository_url>
git pull origin master
```

Инициализируем локальный репозиторий, добавляем ссылку на удаленный репозиторий, вытягиваем себе все изменения

```bash
git add .
git commit -am "initial commit"
git push origin master
```

Добавляем содержимое текущей директории в индекс, коммитим, отправляем в удаленный репозиторий
В строке терминала должен высвечиваться значок `≡`

```bash
[master ≡]
````