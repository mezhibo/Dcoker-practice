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





