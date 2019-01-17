# Работа с Ubuntu

## Установка Ubuntu на Hyper-V

### Установка сетевых инструментов

```bash
sudo apt-get install net-tools          # устанавливаем net-tools
ifconfig                                # получаем сетевые настройки
```

### Установка SSH сервера для удаленного оподключения

```bash
sudo apt-get install openssh-server     # устанавливаем SSH сервер
```

### Подключение к виртуальной машине по SSH

```bash
ssh dmalikov@172.17.216.168
```

### Установка MS SQL Server на Ubuntu

[Официальная документация по установке](https://docs.microsoft.com/ru-ru/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-2017)

[Если что-то пошло не так](https://docs.microsoft.com/ru-ru/sql/linux/sql-server-linux-troubleshooting-guide?view=sql-server-2017)

```bash
sudo systemctl status mssql-server      # проверить статус службы сервера
sudo systemctl stop mssql-server        # остановить службу
sudo systemctl start mssql-server       # запустить службу
sudo systemctl restart mssql-server     # перезапустить службу
```
