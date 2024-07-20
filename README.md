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



