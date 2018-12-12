# Подключение SOAP сервиса к проекту .NET Core

## Иcпользование утилиты dotnet-svcutil

Выполняем команды:

```bash
dotnet add package dotnet-svcutil
dotnet svcutil https://tracking.russianpost.ru/rtm34?wsdl
```

Получаем неожиданную ошибку (для Win10):

```message
Не найден исполняемый файл, соответствующий команде "dotnet-svcutil".
```

Решение найдено:

```message
In your csproj file try replacing "PackageReference" with "DotNetCliToolReference" for the dotnet-svcutil entry. That should let dotnet know that you want to use the package as a cli tool instead of a standard package reference.
```

Подключаем Json.NET

```bash
dotnet add package Newtonsoft.Json
```