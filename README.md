# dockerCommands
**Just tiny list of docker's commands/Шпаргалка по Docker :whale: :new_moon_with_face:**

### Оглавление
1. [Images/ Работа с образами](#images-работа-с-образами)
2. [Containers/ Работа с контейнерами](#containers-работа-с-контейнерами)
3. [Dockerfile/ Создание собственных образов](#dockerfile-создание-собственных-образов)
4. [.dockerignore/ Иселючение файлов и каталогов из образа](#dockerignore-иселючение-файлов-и-каталогов-из-образа)
5. [Other/ Неотсортированные полезные команды](#other-неотсортированные-полезные-команды)
6. [Заключение](#заключение)

### Images/ Работа с образами
1) Скачать образ(Image) можно командой pull:
```
$ docker pull ubuntu
```

2) Посмотреть образы, скаченные на локальную машину, можно командой Images или image ls:
```
$ docker images
$ docker image ls
```

3) Удалить ненужные образы можно с помощью команды rmi:
```
$ docker rmi id_образа
```

4) Удалить все неиспользуемые образы можно командой image prun:
```
$ docker image prun
```

5) Запустить образ(Image) можно командой run:
```
$ docker run имя_образа или id_образа
```
 >Ключ -it ( --interactive --tty) - оставить возможность взаимодействия с контейнером через терминал
```
$ docker run -it имя_образа или id_образа ...
```
 >Ключ -p указание порта -p порт_хоста:порт_контейнера
```
$ docker run -p 3000:3000 имя_образа или id_образа ...
```
>Ключ -d (--detach) - detached mode запуск в фоновом режиме
```
$ docker run -d имя_образа или id_образа ...
```
>Ключ --name задаёт имя контейнеру
```
$ docker run --name=имя_контейнера имя_образа или id_образа ...
```
>Ключ --rm удалит контейнер после его остановки
```
$ docker run --rm имя_образа или id_образа ...
```
>Ключ -e задаёт значение переменным окружения
```
$ docker run -e PORT=80 имя_образа или id_образа ...
```
 
 ### Containers/ Работа с контейнерами
 
1) Посмотреть запущенные контейнеры можно командой ps:
```
$ docker ps -a
```
>(ключ -a показывает вообще все контейнеры, даже приостановленные)

2) Удалить приостановленные контейнеры можно командой rm:
```
$ docker rm имя_контейнера или id_контейнера (можно несколько id)
```

3) Удалить **все** приостановленные контейнеры можно командой container prune:
```
$ docker container prune (Необходимо нажать y(yes) для подтверждения)
```

4) Запуск приостановленного **контейнера** можно осуществить по команде Start:
```
$ docker start имя_контейнера или id_контейнера
```

5) Чтобы подключиться к контейнеру во время работы в нём приложения используется команда attach:
```
$ docker attach имя_контейнера или id_контейнера
```

6) Логи и вывод в консоль сообщений в контейнере осуществляется командой logs
```
$ docker logs имя_контейнера или id_контейнера
```

### Dockerfile/ Создание собственных образов

Инструкции для сборки(создания) собственного образа(Image) docker необходимо записывать в специальном файле Dockerfile (с большой буквы без раcширения файла).

Создаётся частный случай для конкретного приложения.

Образ создаётся на основе существующего образа.
```
FROM название_образа(Image) на основе которого будет приложение.
(Если этот оброз есть локально, то используется локальный образ, если нет, то качается нужный образ.)
```
COPY копирует файлы и папки пользователя в образ. 

COPY .(точка - текущий каталог) - копирует корень проекта (где лежит докер-файл)
Дальше необходимо указать в какое место необходимо скопировать файлы
```
COPY . . (из корня проекта в корень образа)
```
WORKDIR - рабочая дириктория для проекта в образе
```
WORKDIR /app (тогда COPY . . будет копировать файлы в WORKDIR, т.е. в /app)
```

RUN запускает что-то при сборке. Например, если необходимо установить дополнительные пакеты, библиотеки и т.д.

CMD запускается(выполняется) при запуске контейнера. Представляет массив слов команды, например:
```
CMD ["go", "run", "main.go"]
```

EXPOSE указывает какой порт используется для запуска приложения

### Пример Dockerfil'а:
```
FROM node

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

CMD ["node", "app.js"]
```

Сборка собственного образа происходит по команде build:
```
$ docker build . (Если файл Dockerfile расположен в текущей директории)
```
  >Ключ -t позволяет задать имя создаваемому образу а также тэг(по-умолчанию(если не указывать) тэг latest) через :  
  >!!!Важно!!! Имя и тэг необходимо указать в нижнем регистре!!!
```
$ docker build -t newname:newtag .
```

### .dockerignore/ Иселючение файлов и каталогов из образа

Если необходимо исключить какие-то файлы из копируемых в образ  
можно использовать файл .dockerignore

### Пример .dockerignore:
```
node_modules
.git
Dockerfile
.idea
```

### Other/ Неотсортированные полезные команды

1) Чтобы узнать ip виртуальной машины можно воспользоваться командой ip утилиты docker-machine
```
docker-machine ip
```

# Заключение

Это только начало моей работы с Docker. По мере получения мною новых знаний буду добавлять  
команды в данную шпаргалку:alien:

[:point_up:Оглавление](#оглавление)
