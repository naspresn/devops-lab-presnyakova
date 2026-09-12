University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)
Year: 2025/2026
Group: U4225
Author: Преснякова Анастасия Алексеевна
Lab: Lab1
Date of create: 07.09.2026
Date of finished: 14.09.2026

# Лабораторная работа №1. Основы работы с Docker

## Цель работы

Освоить базовые операции Docker: установку, понимание основных команд Docker, опыт работы с готовыми образами и умение запускать и управлять контейнерами.

## Ход работы

### 1. Установка Docker и проверка работоспособности

Docker Desktop установлен с официального сайта. Проверка версии:

```bash
docker --version
```

```
Docker version 29.7.2, build a7dcaa6
```

![Версия Docker](screenshots/01-docker-version.png)

Запуск тестового контейнера:

```bash
docker run hello-world
```

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
58dee6a49ef1: Pull complete
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

Образа `hello-world` на компьютере не было, поэтому Docker сначала скачал его с Docker Hub, а потом запустил контейнер. Сообщение «Hello from Docker!» означает, что установка работает.

![Запуск hello-world](screenshots/02-hello-world.png)

Список скачанных образов:

```bash
docker images
```

```
IMAGE                            ID            DISK USAGE   CONTENT SIZE
hello-world:latest               5dd0d3e6e255       22.6kB         10.3kB
jetbrains/youtrack:2026.2.17765  54d25c6f1003        5.3GB           2GB
python:3.12-slim                 57cd7c3a7a27        205MB        45.6MB
```

![Список образов](screenshots/03-docker-images.png)

Список работающих контейнеров:

```bash
docker ps
```

Вывелся только заголовок таблицы — работающих контейнеров нет. Контейнер `hello-world` напечатал сообщение и сразу закончил работу, поэтому в этом списке его нет.

![Работающие контейнеры](screenshots/04-docker-ps.png)

Список всех контейнеров, включая остановленные:

```bash
docker ps -a
```

```
CONTAINER ID   IMAGE                             COMMAND                  STATUS
0c94c8cd60c9   hello-world                       "/hello"                 Exited (0) 4 minutes ago
0a4cf12e959f   python:3.12-slim                  "bash -c 'pip instal…"   Exited (137) 2 minutes ago
11a1ec4d0734   jetbrains/youtrack:2026.2.17765   "/bin/bash /run.sh"      Exited (0) 2 weeks ago
```

![Все контейнеры](screenshots/05-docker-ps-a.png)

### 2. Работа с готовым образом Ubuntu

Скачивание образа:

```bash
docker pull ubuntu:latest
```

```
latest: Pulling from library/ubuntu
50914c2b24a1: Pull complete
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

![Загрузка образа Ubuntu](screenshots/06-pull-ubuntu.png)

Запуск контейнера в интерактивном режиме:

```bash
docker run -it ubuntu bash
```

Установка пакета внутри контейнера:

```bash
apt update && apt install -y curl
```

```
Setting up curl (8.18.0-1ubuntu2.4) ...
Processing triggers for libc-bin (2.43-2ubuntu2.3) ...
Processing triggers for ca-certificates (20260601~26.04.1) ...
```

![Установка curl](screenshots/07-apt-install-curl.png)

Проверка установки:

```bash
curl --version
```

```
curl 8.18.0 (aarch64-unknown-linux-gnu) libcurl/8.18.0 OpenSSL/3.5.5 zlib/1.3.1 ...
Release-Date: 2026-01-07, security patched: 8.18.0-1ubuntu2.4
```

![Проверка curl](screenshots/08-curl-version.png)

Выход из контейнера:

```bash
exit
```

### 3. Запуск веб-сервера nginx

```bash
docker run -d -p 8080:80 --name web-server nginx:alpine
```

```
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
...
Status: Downloaded newer image for nginx:alpine
8951a146e1f2b19109fd3543be396734d025cae687715ae190bc8b906ba9447d
```


![Запуск nginx](screenshots/09-run-nginx.png)


![Страница nginx в браузере](screenshots/10-nginx-browser.png)

Логи контейнера:

```bash
docker logs web-server
```

```
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/09/07 17:27:53 [notice] 1#1: nginx/1.31.5
2026/09/07 17:27:53 [notice] 1#1: start worker processes
192.168.65.1 - - [07/Sep/2026:17:28:08 +0000] "GET / HTTP/1.1" 200 896 ...
192.168.65.1 - - [07/Sep/2026:17:28:08 +0000] "GET /favicon.ico HTTP/1.1" 404 555 ...
```

![Логи контейнера](screenshots/11-docker-logs.png)

Подключение к работающему контейнеру:

```bash
docker exec -it web-server sh
```

![Подключение к контейнеру nginx](screenshots/12-docker-exec-nginx.png)

### 4. Управление контейнерами

```bash
docker ps
```

```
CONTAINER ID   IMAGE          COMMAND                  STATUS         PORTS
8951a146e1f2   nginx:alpine   "/docker-entrypoint.…"   Up 9 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
```


![Работающий nginx](screenshots/13-docker-ps-nginx.png)

```bash
docker ps -a
```

```
CONTAINER ID   IMAGE          COMMAND                  STATUS
8951a146e1f2   nginx:alpine   "/docker-entrypoint.…"   Up 10 minutes
80a8e5028dc6   ubuntu         "bash"                   Exited (0) 10 minutes ago
0c94c8cd60c9   hello-world    "/hello"                 Exited (0) 22 minutes ago
```


![Все контейнеры](screenshots/14-docker-ps-a-all.png)

Остановка и повторный запуск:

```bash
docker stop web-server
docker start web-server
```

```
web-server
web-server
```


![Остановка и запуск контейнера](screenshots/15-stop-start.png)

Удаление контейнера:

```bash
docker stop web-server
docker rm web-server
```

```
web-server
```

![Удаление контейнера](screenshots/16-docker-rm.png)

Удаление образа:

```bash
docker rmi nginx:alpine
```

```
Untagged: nginx:alpine
Deleted: sha256:72ba65eb42c10344912a84ff42408db7d34f2feb642204570ab8fc5ffd29f1d3
```


![Удаление образа](screenshots/17-docker-rmi.png)

### 5. Работа с томами


```bash
docker volume create my-volume
docker run -it --name volume-test -d -v my-volume:/data ubuntu bash
```

```
my-volume
7aa1ab63ac53a6552770e8a51c0489c55cd954d1fc1aa7b6509c164e931893d5
```

![Создание тома и контейнера](screenshots/18-volume-create-run.png)

Создание файла в томе:

```bash
docker exec -it volume-test bash
echo "Hello from volume" > /data/test.txt
exit
```

![Запись файла в том](screenshots/19-volume-write-file.png)

Удаление контейнера:

```bash
docker stop volume-test
docker rm volume-test
docker ps -a
```

![Удаление контейнера с томом](screenshots/20-volume-rm-container.png)

Создание нового контейнера с тем же томом:

```bash
docker run -it --name volume-test -d -v my-volume:/data ubuntu bash
```

```
29a0201146a0eb89dc052bd73314b705ee34046daeb9543ff3aa31ef8315f32e
```

![Пересоздание контейнера](screenshots/21-volume-recreate.png)

```bash
docker ps
```

```
CONTAINER ID   IMAGE    COMMAND   CREATED          STATUS         NAMES
29a0201146a0   ubuntu   "bash"    23 seconds ago   Up 22 seconds  volume-test
```

![Новый контейнер](screenshots/22-volume-ps.png)

Проверка, что файл сохранился:

```bash
docker exec -it volume-test cat /data/test.txt
```

```
Hello from volume
```

![Проверка файла в томе](screenshots/24-volume-verify.png)

Удаление контейнера после проверки:

```bash
docker stop volume-test
docker rm volume-test
```

![Уборка](screenshots/23-volume-cleanup.png)

```bash
docker volume ls
docker volume rm my-volume
```

## Выводы

В ходе работы установлен Docker, скачаны готовые образы с Docker Hub, запущены контейнеры в интерактивном и фоновом режимах, настроен доступ к веб-серверу nginx через порт 8080, прочитаны логи, выполнены команды внутри работающего контейнера и проверена работа тома.
