
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

### Инструкция к выполению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
2. Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
3. Своё решение к задачам оформите в вашем GitHub репозитории в формате markdown!!!
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- скачайте образ nginx:1.29.0;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .

Решение:
- Установка docker и docker compose:
  sudo apt update
  sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
- Скачать образ nginx:1.29.0:
  docker pull nginx:1.29.0
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы:
  cat Dockerfile
  FROM nginx:1.29.0
  COPY index.html /usr/share/nginx/html/index.html
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0:
  docker login
  docker build -t timurdevlikamov/custom-nginx:1.0.0 .
- Предоставьте ответ в виде ссылки:
  https://hub.docker.com/repository/docker/timurdevlikamov/custom-nginx/general


## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

Решение:
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
  docker push timurdevlikamov/custom-nginx:1.0.0
  docker run -d --name devlikamovtimur-nginx-t2 -p 127.0.0.1:8080:80 timurdevlikamov/custom-nginx:1.0.0
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
   docker rename devlikamovtimur-nginx-t2 custom-nginx-t2
3. Вывод выполнения в приложенном к решению задачи скриншоте
4. curl -i http://127.0.0.1:8080


## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

Решение:
1. docker exec -it custom-nginx-t2 bash
2. docker run -it --name custom-nginx-t2 -p 127.0.0.1:8080:80 timurdevlikamov/custom-nginx:1.0.0
3. В пункте 2-а мы запустил и подключились к контейнеру терминалом который привязан к главному процессу контейнера, соотвественно при вызове сизнала Ctrl+C (сигнал прерывания процесса) мы прерываем процесс под которым запущен процесс контейнера, в итоге контейнер остановился.
4. docker restart custom-nginx-t2
5. docker run -it custom-nginx-t2 bash (у нас же ещё не запушен контейнер, если он запущен то docker exec -it custom-nginx-t2 bash)
6. apt-get install vim
7. vi /etc/nginx/conf.d/default.conf
   cat /etc/nginx/conf.d/default.conf | grep listen
   listen 81;
8. nginx -s reload
   curl http://127.0.0.1:80 ; curl http://127.0.0.1:81
    curl: (7) Failed to connect to 127.0.0.1 port 80 after 0 ms: Couldn't connect to server
    <! DOCTYPE html>
    khtml>
    <head>
    Hey, Netology
    </head>
    <body>
    <h1>I will be DevOps Engineer !< /h1>
    </body>
    </html>
9. exit
10. Изначально мы настраивали port forwarding на 127.0.0.1:8080:80, после изменения конфига nginx на 81 порт ожидаемо страница не грузиться по порту 8080
12. docker rm -f custom-nginx-t2 

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

Решение:
- docker run -d -v "$(pwd) :/data" debian
  docker run -d -v "$(pwd) :/data" centos:7
- ls /data/
Скрины к выводу файлов приложены в решении задачи.

## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

Решение:
1. mkdir -p /tmp/netology/docker/task5
   vi /tmp/netology/docker/task5/compose.yaml
   vi /tmp/netology/docker/task5/docker-compose.yaml
   docker compose up -d
   По итогу запустился /tmp/netology/docker/task5/compose.yaml
   WARN[0000] Using /tmp/netology/docker/task5/compose.yaml
   так как он используется в качестве дефолтного файла для запуска, docker-compose.yaml может использоваться только в качестве совестимости в легасиэ
2. Добавил в соотвествии с документацией инструкцию:
   include:
    - docker-compose. yaml
3. docker push localhost:5000/custom-nginx:latest
Скрины по заданиям 4,5,6 приложены к решению данной задачи.
7. docker compose up -d
  WARN[0000] /tmp/netology/docker/task5/docker-compose.yaml: the attribute 'version' is obsolete, it will be ignored, please remove it to avoid potential confusion
  WARN[0000] Found orphan containers ([task5-portainer-1]) for this project. If you removed or renamed this service in your compose file, you can run this command with the -- remove-
  orphans flag to clean it up.
  Как понимаю WARN - "Found orphan containers" выводит информацию об осиротевшем котнейнере "task5-portainer-1" и предлагает применить флаг -- remove-orphans для его очистки
  По итогу выполнил:
  docker compose up -d -- remove-orphans

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


