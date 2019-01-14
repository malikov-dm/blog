# Шпаргалка по использованию Git 1

## Установка

[Официальный сайт Git](https://git-scm.com/downloads)

Linux:

```bash
apt-get install git
```

Windows:

[Установка расширения PowerShell для работы с Git (posh-git)](https://github.com/dahlbyk/posh-git#installation)

Если что-то идет не так:

```powershell
remove-module PowerShellGet,PackageManagement -force
Import-Module -Name PowerShellGet -Force;
Import-PackageProvider -Name PowerShellGet -Force
PowerShellGet\Install-Module posh-git -Scope CurrentUser -AllowPrerelease -Force
```

Обновление:

```powershell
git update-git-for-windows
```

## Конфигурация и подготовка к работе

### Настройка конфига и алиасов

***Открыть конфиг на редактирование***

```bash
git config --global -e
```

***Установка NotePad++ в качестве редактора по умолчанию***

32-bit

```bash
git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

64-bit

```bash
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

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

> Если профиль не откроется (возможно, переменная `$profile` указана неверно), его нужно задать явно.
> В моем случае не было папки `WindowsPowerShell`, но была папка `PowerShell`, что я и указал в пути до профиля:
>
> ```powershell
> $profile = "C:\Users\Username\Documents\PowerShell\Microsoft.PowerShell_profile.ps1"
> ```
>
> После этого профиль работает корректно

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

***Подключение файлов в конфиге***

В целях уменьшения количества дублирующих конфигов можно использовать их повторно.

Конфиг можно подключить по ссылке в другом конфиге:

```bash
git config --add include.path ../gitconfig
```

Можно создать конфиг в корне проекта, чтобы делиться им с другими разработчиками (так как системные, глобальные и локальные конфиги в репозиторий не попадают), подключить его вышеуказанной программой и использовать один конфиг в проекте.

[Инициализация репозитория, подключение к удаленному репозиторию, история >](git-cheat-sheet-2.md)