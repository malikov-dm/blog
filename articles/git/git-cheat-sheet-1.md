# Шпаргалка по использованию Git 1

## Конфигурация и подготовка к работе

### Настройка конфига и алиасов

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

***Алиасы***

```bash
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch
git config --global alias.hist "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short"
git config --global alias.type 'cat-file -t'
git config --global alias.dump 'cat-file -p'
```

Если `PowerShell` некорректно отображает кириллицу в тексте комментариев к коммиту, нужно установить переменную среды:

```powershell
set LC_ALL=C.UTF-8
```

Лучше будет даже добавить ее перманентно: `Панель управления -> Система и безопасность -> Система -> Дополнительные параметры среды -> Переменные среды... -> Создать...`

Если мы используем `Powershell` и расширение `posh-git` для него, алиасы для оболочки просто так создать не получится.
Нужно создавать отдельный скрипт с функциями, которые будут реализовывать эти алиасы.
Для примера, создадим алиас `go` для команды `git checkout`, чтобы иметь возможность вызывать эту команду вот так:

```bash
go master       #переключились на master
go feature      #переключились на feature
```

Для этого откроем файл профиля `Powershell` в блокноте:

```powershell
notepad $PROFILE
```

и добавим в конец следующий код:

```powershell
## posh-git-aliases
function go() {
    git checkout $args[0]
}
## end posh-git-aliases
```

Готово, теперь в консоли `Powershell` работает алиас `go`.
Также, при большом количестве алиасов имеет смысл вынести их в отдельный файл и добавить в файл конфига на него ссылку:

```powershell
. "<path_to_file>\posh-git-aliases.ps1"
```

Не забудьте точку и пробел после нее.

***Алиасы для команд***

```bash
alias gs='git status '
alias ga='git add '
alias gb='git branch '
alias gc='git commit'
alias gd='git diff'
alias go='git checkout '
alias gk='gitk --all&'
alias gx='gitx --all'

alias got='git '
alias get='git '
```

В терминале `Git` их можно использовать непосредственно, для `Powershell` придется написать функции как я показывал выше.