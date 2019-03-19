# Еще один гайд по использованию Docker и Nginx при разработке веб-приложений на .NET Core

## Создание решения

- 1. Создаем решение

- 2. Билдим и запускаем оркестр

```powershell
docker-compose build
docker kill
docker rm
docker ps -a
docker-compose up

docker-compose down -v

docker exec -it dev_nginx /bin/bash
```

- 3. Проверяем работоспособность

localhost/

Если посмотреть на заголовок любого файла, который скачал браузер при входе на сервер, можно увидеть там nginx

Обратить внимание на nginx.conf в котором прокси пасс указан как http://dev_webapp - докер сам определяет IP этого контейнера. Говорят, через /etc/hosts на сервере nginx, но я не нашел там этой записи - магия.

Удалить все подряд:

```powershell
docker image ls
docker system prune -a --volumes
```

```bash
apt-get install openssl
```