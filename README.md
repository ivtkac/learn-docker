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

## Принципи

> [!quote] Працює, безпечне, білдиться швидко

S - безпека, B - швидкість збірки, C - чіткість

- (SBC) версії, що залежать від виводу
- (SBC) базові образи (або основні + другорядні, або хеш SHA256)
- (SC) системні залежності
- (SC) залежності програм
- (SB) використовувати малі + безпечні базові образи
- (BC) захищати кеш шарів
- (B) упорядковувати команди за частотою змін
- (B) `COPY` файл вимог до залежностей -> встановити залежності -> скопіювати залишковий вихідний код
- (B) Використовувати монтування кешу
- (B) Використовувати `COPY --link`
- (BC) Об'єднувати кроки, які завжди пов'язані (використовувати heredoc для покращення акуратності)

> [!quote] Зробіть так, щоб це працювало, зробіть це БЕЗПЕЧНО, зробіть збірку швидкою

- (SC) бути explicit
- (C) встановити робочий каталог за допомогою `WORKDIR`
- (C) вказати стандартний порт за допомогою `EXPOSE`
- (SC) Встановити змінні середовища за замовчуванням за допомогою `ENV`
- (SBC) Уникати непотрібних файлів
- (SBC) Використовувати .dockerignore
- (SBC) Копіювати певні файли
- (S) Використовувати не root-користувача
- (SBC) Встановлювати лише виробничі залежності
- (S) Уникати витоку конфіденційної інформації
- (SB) Використовувати багатоетапні збірки
