# Работа с Amazon Web Services с точки зрения разработчика

## Подключение через SSH

```bash
ssh -i 'c:\Users\USER1\.ssh\AWSMSSQLUbuntu.pem' ubuntu@54.202.162.108
```

Имя пользователя - ubuntu (может быть другим)

## Установка MS SQL Server 2017 на Ubuntu

info: <https://docs.microsoft.com/ru-ru/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-2017>

ec2-54-202-162-108.us-west-2.compute.amazonaws.com      54.202.162.108

sqlcmd -S 54.202.162.108 -U SA

### После установки в Security Group добавить Inbound Rule для MS SQL Server

MS SQL
TCP
1433
0.0.0.0/0

SSH
TCP
22
0.0.0.0/0

ec2-34-211-217-110.us-west-2.compute.amazonaws.com
34.211.217.110

```bash
ssh -i 'c:\Users\USER1\.ssh\AWSMicroUbuntu.pem' ubuntu@34.211.217.110
```

## Установка .NET Core на Ubuntu

1: <https://docs.microsoft.com/ru-ru/dotnet/core/linux-prerequisites?tabs=netcore2x>  
2: <https://docs.microsoft.com/ru-ru/dotnet/core/tools/dotnet-install-script>  
3: <https://www.microsoft.com/net/download/linux-package-manager/ubuntu18-04/runtime-2.1.2>  

```bash
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo add-apt-repository universe
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install aspnetcore-runtime-2.1
sudo apt-get install dotnet-sdk-2.1.200
```

Устанавливаем nginx

```bash
sudo apt-get install nginx
```

Создаем конфиг и открываем его

```bash
sudo touch /etc/nginx/sites-available/aspnetcore.conf
```

Открываем файл на запись

```bash
sudo chmod u+w /etc/nginx/sites-available/aspnetcore.conf
```

Открываем файл в редакторе

```bash
sudo nano /etc/nginx/sites-available/aspnetcore.conf
```

и добавляем конфиг:

```config
server {
    listen 80;
    listen [::]:80;
    server_name dmalikov.rdvb.ru;
    location / {
        proxy_pass https://localhost:5001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Добавляем символическую ссылку

```bash
sudo ln -s /etc/nginx/sites-available/aspnetcore.conf /etc/nginx/sites-enabled/aspnetcore.conf
```

Запускаем nginx прокси сервер

```bash
sudo service nginx start
```

Перезагрузить nginx можно командой  

```bash
sudo service nginx restart
```

Создаем и запускаем приложение dotnet core

```bash
dotnet new mvc
dotnet run
```

## Публикация своего приложения, собранного под Win

Упаковать приложение в архив  
Разместить архив где-нибудь в интернете  
Cкачать его при помощи wget <ссылка на архив>  
Распаковать архив  
tar -xvf <имя архива>  
Все, приложение расположено на сервере. Его можно запускать.

Запустить приложение

```bash
dotnet <имя приложения>.dll
```

Приложение запущено на 5000 порту (по умолчанию), nginx пересылает на него запросы с 80 порта, все работает.

Далее, можно запустить приложение как сервис (у линуксоидов - демон)

В каталоге подсистемы инициализации (systemd) создаем конфиг:

```bash
sudo nano /etc/systemd/system/rdvb.service
```

с содержимым:

```config
[Unit]
Description=dmalikov personal page dotnet core web application

[Service]
WorkingDirectory=/home/ubuntu/rdvb
ExecStart=/home/ubuntu/rdvb/Rdvb.dll
Restart=always
RestartSec=10
SyslogIdentifier=dotnet-rdvb
User=ubuntu
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target
```

Запускаем демон

```bash
sudo systemctl enable rdvb.service
sudo systemctl start rdvb.service
```

Проверяем статус

```bash
sudo systemctl status rdvb.service
```

Проверка работоспособности сайта извне через PS

```bash
curl http://ya.ru | % {$_.StatusCode}
```