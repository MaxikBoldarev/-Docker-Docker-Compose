# Docker-Docker-Compose
## Задача 1
Сценарий выполнения задачи:

Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: {"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}
Зарегистрируйтесь и создайте публичный репозиторий с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
скачайте образ nginx:1.29.0;
Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП).
Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .
  
# Ответ : https://hub.docker.com/repository/docker/maxik1394/custom-nginx/general

## Задача 2
Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
имя контейнера "ФИО-custom-nginx-t2"
контейнер работает в фоне
контейнер опубликован на порту хост системы 127.0.0.1:8080
Не удаляя, переименуйте контейнер в "custom-nginx-t2"
Выполните команду date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

# Ответ : 
<img width="2233" height="455" alt="image" src="https://github.com/user-attachments/assets/69fb9c14-368b-46ca-82f6-14e0f5f20204" />

## Задача 3
Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
Выполните docker ps -a и объясните своими словами почему контейнер остановился.
Перезапустите контейнер
Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
Запомните(!) и выполните команду nginx -s reload, а затем внутри контейнера curl http://127.0.0.1:80 ; curl http://127.0.0.1:81.
Выйдите из контейнера, набрав в консоли exit или Ctrl-D.
Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8080 , docker port custom-nginx-t2, curl http://127.0.0.1:8080. Кратко объясните суть возникшей проблемы.
Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. пример источника
Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

# Ответ : 
<img width="1430" height="757" alt="image" src="https://github.com/user-attachments/assets/a8a90d5a-91bb-48e8-b31e-42071e9cb9e6" />
<img width="1851" height="645" alt="image" src="https://github.com/user-attachments/assets/a4a08aa8-f236-44fa-9e04-9baf16c977be" />
<img width="955" height="708" alt="image" src="https://github.com/user-attachments/assets/2a6c451a-caf2-4228-a594-4838c4b3065c" />

Объяснение, почему контейнер остановился:
Когда мы подключились к контейнеру через docker attach, мы присоединились к главному процессу контейнера (nginx). Нажатие Ctrl-C отправило сигнал SIGINT главному процессу, что привело к его завершению. Поскольку nginx был единственным процессом, поддерживающим контейнер в рабочем состоянии, контейнер остановился. Docker контейнер работает только пока работает его главный процесс (PID 1).

Проверка проблемы снаружи контейнера:
Возникла проблема несоответствия портов. При запуске контейнера мы настроили проброс портов -p 127.0.0.1:8080:80, что означает: порт 8080 хоста → порт 80 контейнера. Однако внутри контейнера мы изменили nginx так, что он теперь слушает порт 81 вместо 80. 
Проброс портов остался прежним (8080→80), но nginx больше не слушает порт 80 внутри контейнера, поэтому при обращении к http://127.0.0.1:8080 с хоста мы получаем ошибку "Connection refused" или "Empty reply from server". Трафик приходит на порт 80 контейнера, но там никто не слушает.

## Дополнительное задание: Исправление без удаления контейнера
Переводил с 80 на 81 порт.
<img width="2479" height="738" alt="image" src="https://github.com/user-attachments/assets/f2098a33-d8b2-4fb1-a10e-9c33a8048fb4" />
<img width="1546" height="293" alt="image" src="https://github.com/user-attachments/assets/079d88f3-ae4e-4def-b6a2-e93f0127185d" />

## Задача 4
Запустите первый контейнер из образа centos c любым тегом в фоновом режиме, подключив папку текущий рабочий каталог $(pwd) на хостовой машине в /data контейнера, используя ключ -v.
Запустите второй контейнер из образа debian в фоновом режиме, подключив текущий рабочий каталог $(pwd) в /data контейнера.
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
Добавьте ещё один файл в текущий каталог $(pwd) на хостовой машине.
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

# Ответ : 
<img width="1688" height="1311" alt="image" src="https://github.com/user-attachments/assets/17178126-2c50-445a-8428-9d416ec6c352" />
<img width="695" height="182" alt="image" src="https://github.com/user-attachments/assets/e7bcedb8-ffe9-42d9-8f1b-a9adf945feb5" />

## Задача 5
Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него. "compose.yaml" с содержимым:
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
"docker-compose.yaml" с содержимым:

version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/

Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)

Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

Удалите любой из манифестов компоуза(например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

# Ответ : 
<img width="956" height="767" alt="image" src="https://github.com/user-attachments/assets/896d6d58-5880-4d95-96b5-b8e21dd1188f" />
<img width="1592" height="439" alt="image" src="https://github.com/user-attachments/assets/b27822fa-c4e2-49b4-bced-f688522b6da4" />
Был запущен файл compose.yaml, потому что согласно документации Docker Compose, порядок приоритета файлов следующий:

1. compose.yaml (наивысший приоритет)
2. compose.yml
3. docker-compose.yaml
4. docker-compose.yml (наименьший приоритет)

Docker Compose ищет файлы в этом порядке и использует первый найденный файл. Поскольку compose.yaml имеет наивысший приоритет, он был запущен, а docker-compose.yaml был проигнорирован.

<img width="1646" height="1176" alt="image" src="https://github.com/user-attachments/assets/1dcf1b9f-11dd-4f87-858c-742e0d9129c0" />
<img width="1184" height="380" alt="image" src="https://github.com/user-attachments/assets/6fce1425-028b-42da-9471-157e35ed7d8b" />
<img width="2167" height="406" alt="image" src="https://github.com/user-attachments/assets/9a5f9483-e112-40e9-b86c-a9f41e5c8e55" />
<img width="1292" height="1231" alt="image" src="https://github.com/user-attachments/assets/84dd1300-97c0-4576-b263-9e1d9afcea3f" />
<img width="1932" height="236" alt="image" src="https://github.com/user-attachments/assets/f6515b50-19bc-4c13-a15f-242c63ce16fb" />
Суть предупреждения:
- Docker Compose обнаружил, что основной файл compose.yaml был удален
- Теперь используется docker-compose.yaml (следующий в приоритете)
- Предупреждение о том, что атрибут version устарел в новых версиях Docker Compose (v2.x)
- В Compose v2 версия файла определяется автоматически, и указывать version: не обязательно
<img width="2000" height="145" alt="image" src="https://github.com/user-attachments/assets/4ec0e4b4-d100-43dc-bb1c-1a471dba27fb" />












