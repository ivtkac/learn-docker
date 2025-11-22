# синій кит повертається, збуди мене о 4:20

## Playground

### run whalesay
```bash
docker run ok7827125/whalesay:mytag1 cowsay "Hello Docker"
```

### run postgresql
```bash
docker run --env POSTGRES_PASSWORD=foobarbaz --publish 5432:5432 postgres:15.1-alpine
```

## Встановлення залежностей

```bash
docker run --interactive --tty --rm ubuntu:22.04
# Try to ping
ping google.com -c1 # `ping`: command not found

apt update
apt install iputils-ping --yes

ping google.com -c1
exit
```

Після виходу, команди `ping` не існує, якщо ми спробуємо знову запустити цей образ.

Ми можемо дати ім'я контейнеру для перевикористання

```bash
docker run -it --name my-ubuntu-container ubuntu:22.04
# Try to ping
ping google.com -c1 # `ping`: command not found

apt update
apt install iputils-ping --yes

ping google.com -c1
exit

# List all containers
docker container ps -a  | grep my-ubuntu-container
docker container inspect my-ubuntu-container

# Restart the container and attach to running shell
docker start my-ubuntu-container
docker attach my-ubuntu-container

# Test ping
ping google.com -c 1 # IT RUNS!!!
```

Як правило краще не залежити від контейнера для збереження даних, тому для залежностей як раніше, ми можемо включити це у образ:

```shell
docker build --tag my-ubuntu-image -<<EOF
FROM ubuntu:22.04
RUN apt update && apt install iputils-ping --yes
EOF

# Запустити контейнер
docker run -it --rm my-ubuntu-image
# try ping
ping google.com -c1
```

`FROM... RUN...` це частини, які називаються `Dockerfile` і використовуються, щоб визначити ЯК білдити образ контейнера.

## Збереження даних додатком

Щоб наші додатки могли безпечно зберігати дані (БД, user data, ...) навіть якщо контейнер знищений або перестворений. Для цього використовувати `Volume` і `mounts`.

```bash
docker run -it --rm ubuntu:22.04

mkdir my-data

echo "Hello from the container!" > /my-data/hello.txt

cat my-data/hello.txt
exit

# There is no anymore our file :(
```

### 1. Volume mounts

```bash
docker volume create my-volume
docker run -it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04
# Similar but shorter
docker run -it --rm my-volume:/my-data ubuntu:22.04
```

Створити контейнер і замаунтити існуючий волюм для збережання файла

```bash
docker run -it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04
```

### 2. Bind mounts

Easy manage, easy visibility, but low performance than volumes

```bash
docker run -it --rm --mount type=bind,source="${PWD}"/my-data,destination=/my-data ubuntu:22.04
```
