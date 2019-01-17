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