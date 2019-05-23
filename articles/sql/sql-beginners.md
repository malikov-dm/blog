# Развертывание SQL сервер на Linux

## Установка Ubuntu Server

## Установка MS SQL Server for Linux

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
sudo apt-get update
sudo apt-get install -y mssql-server
```

После завершения установки запустить скрипт настройки и следовать инструкциям на экране

```bash
sudo /opt/mssql/bin/mssql-conf setup
```

## Установка тестовых БД

```bash
wget https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2017.bak
wget https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2017.bak
``` 
