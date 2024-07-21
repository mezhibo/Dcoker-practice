**Задача 0**

Убедитесь что у вас НЕ(!) установлен docker-compose, для этого получите следующую ошибку от команды docker-compose --version
Command 'docker-compose' not found, but can be installed with:

sudo snap install docker          # version 24.0.5, or
sudo apt  install docker-compose  # version 1.25.0-1

See 'snap info docker' for additional versions.
В случае наличия установленного в системе docker-compose - удалите его.
2. Убедитесь что у вас УСТАНОВЛЕН docker compose(без тире) версии не менее v2.24.X, для это выполните команду docker compose version

**Решение 0**

Убеждаемся чьо у нас в ситсеме нет docker-compose

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/45158571f7898acabc693590627acc45ec0f0bf7/IMG/1.jpg)

Зато есть docker compose 

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/45158571f7898acabc693590627acc45ec0f0bf7/IMG/2.jpg)



**Задача 1**

1) Сделайте в своем github пространстве fork репозитория https://github.com/netology-code/shvirtd-example-python/blob/main/README.md.

2) Создайте файл с именем Dockerfile.python для сборки данного проекта. Используйте базовый образ python:3.9-slim. Протестируйте корректность сборки. Не забудьте dockerignore.

3) (Необязательная часть, *) Изучите инструкцию в проекте и запустите web-приложение без использования docker в venv. (Mysql БД можно запустить в docker run).

4) (Необязательная часть, *) По образцу предоставленного python кода внесите в него исправление для управления названием используемой таблицы через ENV переменную.


**Решение 1** 

Форкаем себе репку и создаем новый фаил Dockerfile.python со следующим содержимым

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/45158571f7898acabc693590627acc45ec0f0bf7/IMG/3.jpg)


Далее билдим свой контейнер 

sudo docker build  -f Dockerfile.python -t mezhibo_docker .

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/45158571f7898acabc693590627acc45ec0f0bf7/IMG/4.jpg)


Смотрим что собранный контейнер появился в списке имеджей 

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/45158571f7898acabc693590627acc45ec0f0bf7/IMG/5.jpg)



**Задача 2 (*)**

1) Создайте в yandex cloud container registry с именем "test" с помощью "yc tool" . Инструкция

2) Настройте аутентификацию вашего локального docker в yandex container registry.

3) Соберите и залейте в него образ с python приложением из задания №1.

4) Просканируйте образ на уязвимости.

5) В качестве ответа приложите отчет сканирования.



**Решение 2**

Подключаем докер реджистри к YC 

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/c19a9f6ac135dbafad3db23c267f3531f86e4d32/IMG/6.jpg)

Билдим из тех же исходников новый docker image

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/c7960d98ae6d71c17d9210f42bf8aad22b0fa431/IMG/7.jpg)


Помечаем тэгом и пушим к себе в клауд

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/c7960d98ae6d71c17d9210f42bf8aad22b0fa431/IMG/8.jpg)


ХОБА! а вот он наш ящичек 

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/b4ff2b64e464d2fe1f156ed57344fe4cc13e2d28/IMG/9.jpg)


Теперь посканируем его, и видим что он весь "дырявый" и надо будет ставит заплатки, иначе такое в прод никто не пустит

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/5af953b53e390b53e376151087966142bd03a9bc/IMG/10.jpg)

Теперь можем посмотерть более подробный вывод через YC, увидим все CVE и теперь можем погуглить как фиксить данную уязвимость

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/5af953b53e390b53e376151087966142bd03a9bc/IMG/11.jpg)



**Задача 3**

1) Создайте файл compose.yaml. Опишите в нем следующие сервисы:

- web. Образ приложения должен ИЛИ собираться при запуске compose из файла Dockerfile.python ИЛИ скачиваться из yandex cloud container registry(из задание №2 со *). Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.5. Сервис должен всегда перезапускаться в случае ошибок. Передайте необходимые ENV-переменные для подключения к Mysql базе данных по сетевому имени сервиса web

- db. image=mysql:8. Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.10. Явно перезапуск сервиса в случае ошибок. Передайте необходимые ENV-переменные для создания: пароля root пользователя, создания базы данных, пользователя и пароля для web-приложения.Обязательно используйте .env file для назначения секретных ENV-переменных!

2) Запустите проект локально с помощью docker compose , добейтесь его стабильной работы.Протестируйте приложение с помощью команд curl -L http://127.0.0.1:8080 и curl -L http://127.0.0.1:8090.

3) Подключитесь к БД mysql с помощью команды docker exec <имя_контейнера> mysql -uroot -p<пароль root-пользователя> . Введите последовательно команды (не забываем в конце символ ; ): show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;.

4) Остановите проект. В качестве ответа приложите скриншот sql-запроса.


**Решение 3**


создаем файлик compose.yml
```
'version: '3.8'
services:

  web:
    build:
      context: ./                   # путь до докерфайла
      dockerfile: Dockerfile.python # имя докерфайла
    container_name: web             # имя контейнера
    ports:                          # проброс портов
      - '5000:5000'                 # номер порта
    restart: always                 # перезапуск контейнера
    env_file: .env                  # файл с переменными
    environment:                    # блок переменных
      - TZ=Europe/Moscow            # установка часового пояса МСК
      - DB_HOST=db
      - DB_USER=app
      - DB_PASSWORD:${MYSQL_PASSWORD}
      - DB_NAME=example
    networks:
      backend:                      # добавить в сеть backend
       ipv4_address: 172.20.0.5     # статический IPv4

  db:
    image: mysql:8.0   # версия снимка
    container_name: db # имя контейнера
    ports:
      - '3306:3306'
    restart: always
    env_file: .env
    volumes:           # том и проброс файла в директории
      - ./mysql/my.conf:/etc/mysql/my.cnf:ro
      - mysql_data:/var/lib/mysql
    environment:
      # Все параметры описываем в файле .env в папке проекта
      - TZ=Europe/Moscow
      - MYSQL_ROOT_HOST="%"
      - MYSQL_ROOT_PASSWORD:${MYSQL_ROOT_PASSWORD}
      - MYSQL_USER:${MYSQL_USER}
      - MYSQL_PASSWORD:${MYSQL_PASSWORD}
      - MYSQL_ROOT_PASSWORD:${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE:${MYSQL_DATABASE}
    networks:
      backend:
        ipv4_address: 172.20.0.10

  reverse-proxy:
    image: haproxy
    container_name: reverse-proxy
    restart: always
    ports:
    - '127.0.0.1:8080:8080'
    volumes:
    - ./haproxy/reverse/haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg:rw
    networks:
      backend:
        ipv4_address: 172.20.0.11

  ingress-proxy:
    image: nginx:latest
    container_name: ingress-proxy
    restart: always
    volumes:
    - ./nginx/ingress/default.conf:/etc/nginx/conf.d/default.conf:rw
    - ./nginx/ingress/nginx.conf:/etc/nginx/nginx.conf:rw
    network_mode: host

networks:            # создание сети
  backend:           # название сети контейнеров
    driver: bridge   # тип драйвера сети
    ipam:            # описание параметров сети
      config:
        - subnet: 172.20.0.0/24 # подсеть
          gateway: 172.20.0.1   # шлюз

volumes:
  mysql_data: {}'

```

Теперь делаем docker compose up -d

Видим что у нас пошла сборка 

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/9bd5cac42a4b63dbe12e52076a485f95f7df1431/IMG/12.jpg)


Сборка удачная, теперь посмотрим работают ли контейнеры, видим что все хорошо


![alt text](https://github.com/mezhibo/Dcoker-practice/blob/9bd5cac42a4b63dbe12e52076a485f95f7df1431/IMG/13.jpg)


Теперь подергаем через curl наш ingress proxy

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/169e8c4fdf3b030615c53ed0f8b565f1ace12abc/IMG/14.jpg)


Дальше провалимся в наш контейнер с базой данных, посмотрим список баз, и выберем нашу дефолтную example, в котору по идее должны были записываться запросы, ну сейчас и проверим


![alt text](https://github.com/mezhibo/Dcoker-practice/blob/169e8c4fdf3b030615c53ed0f8b565f1ace12abc/IMG/15.jpg)


Выбираем базу, таблицу и видим все наши запросы через curl 


![alt text](https://github.com/mezhibo/Dcoker-practice/blob/169e8c4fdf3b030615c53ed0f8b565f1ace12abc/IMG/16.jpg)


Теперь сохраняем все изменения и пушим их в форкнутую к  себе репозиторию, для выолнения следующего задания.


![alt text](https://github.com/mezhibo/Dcoker-practice/blob/169e8c4fdf3b030615c53ed0f8b565f1ace12abc/IMG/17.jpg)




**Задача 4**

1) Запустите в Yandex Cloud ВМ (вам хватит 2 Гб Ram).

2) Подключитесь к Вм по ssh и установите docker.

3) Напишите bash-скрипт, который скачает ваш fork-репозиторий в каталог /opt и запустит проект целиком.

4) Зайдите на сайт проверки http подключений, например(или аналогичный): https://check-host.net/check-http и запустите проверку вашего сервиса http://<внешний_IP-адрес_вашей_ВМ>:8090. Таким образом трафик будет направлен в ingress-proxy.

5) (Необязательная часть) Дополнительно настройте remote ssh context к вашему серверу. Отобразите список контекстов и результат удаленного выполнения docker ps -a

6) В качестве ответа повторите sql-запрос и приложите скриншот с данного сервера, bash-скрипт и ссылку на fork-репозиторий.



**Решение 4**


Создадим машинку


![alt text](https://github.com/mezhibo/Dcoker-practice/blob/57cfbdda3f54d28f3a004bff1e6e2ad847ae616f/IMG/18.jpg)


Напишем незатейливый скриптец для скачивания моей репы с правильными данными для выполнения компоса, и дальнейшего компоса, и сразу запустим его

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/57cfbdda3f54d28f3a004bff1e6e2ad847ae616f/IMG/19.jpg)



Видим что все запустилось успешно 

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/57cfbdda3f54d28f3a004bff1e6e2ad847ae616f/IMG/20.jpg)


И теперь идем на портал https://check-host.net/check-http чтобы проверить как наше приложение принмает коннекты

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/57cfbdda3f54d28f3a004bff1e6e2ad847ae616f/IMG/21.jpg)



Делаем массоый запрос на наш сервер

![alt text](https://github.com/mezhibo/Dcoker-practice/blob/0fffd79b0b1d9b7a8601b7e1c435246fd14d19fc/IMG/22.jpg)


Возвращаемся в наш контенйер с нашей базой, и смотрим активность


![alt text](https://github.com/mezhibo/Dcoker-practice/blob/0fffd79b0b1d9b7a8601b7e1c435246fd14d19fc/IMG/23.jpg)



Видим что производились запросы к нашему веб серверу из глобальной сети 


Вот незатейливый скриптец 

'''
#!/bin/bash
echo "Cloning the project from GitHub"
  git clone https://github.com/mezhibo/shvirtd-example-python.git
echo "Done"

echo "Entering the project directory"
  cd shvirtd-example-python
echo "Done"

echo "Creating docker containers: db, app, proxy and nginx"
  sudo docker compose up -d
echo "Done"

echo "List of containers"
  sudo docker ps



'''


[А вот ссылка на форкнутую, но уже верно отредактированную репу](https://github.com/mezhibo/shvirtd-example-python.git)



**Задача 5 (*)**

Напишите и задеплойте на вашу облачную ВМ bash скрипт, который произведет резервное копирование БД mysql в директорию "/opt/backup" с помощью запуска в сети "backend" контейнера из образа schnitzler/mysqldump при помощи docker run ... команды. Подсказка: "документация образа."

Протестируйте ручной запуск

Настройте выполнение скрипта раз в 1 минуту через cron, crontab или systemctl timer. Придумайте способ не светить логин/пароль в git!!

Предоставьте скрипт, cron-task и скриншот с несколькими резервными копиями в "/opt/backup"

Задание необязательное, пропустим



**Задача 6 (*) **

Скачайте docker образ hashicorp/terraform:latest и скопируйте бинарный файл /bin/terraform на свою локальную машину, используя dive и docker save. Предоставьте скриншоты действий .

**Задача 6.1**

Добейтесь аналогичного результата, используя docker cp.

Предоставьте скриншоты действий .


**Решение 6**








