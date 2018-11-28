# Разработка веб-приложения на .NET Core

## Создание решения и проекта

В целом, обычная инициализация решения и проекта.
В качестве шаблона выбрал MVC с авторизацией

### Проблема номер один

При запуске проекта приложение не смогло создать БД, вследствие этого при попытке зарегать пользователя произошла ошибка

```error
Microsoft.EntityFrameworkCore.Database.Connection[20004]
      An error occurred using the connection to database 'aspnet-RdvbDotNetCore-457A7CC3-2032-4078-A4CE-4C56F9035E32' on server '(localdb)\mssqllocaldb'.
System.Data.SqlClient.SqlException (0x80131904): Cannot open database "aspnet-RdvbDotNetCore-457A7CC3-2032-4078-A4CE-4C56F9035E32" requested by the login. The login failed.
Login failed for user...
```