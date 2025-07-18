# Foodgram

Foodgram - продуктовый помощник. Это онлайн-сервис и REST API для него, позволяющий пользователям делиться рецептами, подписываться на других пользователей, добавлять рецепты в «Избранное» и формировать списки покупок. Например, пользователь может найти рецепт борща, добавить его в избранное, подписаться на автора и скачать список ингредиентов в формате .txt для похода в магазин.
Проект реализован на Django и DjangoRestFramework с использованием базы данных PostgreSQL. API реализован с использованием HTTP методов GET, POST, PUT, DELETE и аутентификации на основе JWT. Документация к API написана с использованием Redoc.
В заключение, Foodgram - это полноценный продуктовый сервис, демонстрирующий мои навыки в разработке backend-приложений на Python с использованием Django и DjangoRestFramework.


Особенностью реализации является использование Docker для контейнеризации проекта. Образы foodgram_frontend и foodgram_backend опубликованы на DockerHub. Реализован CI/CD workflow с использованием GitHub Actions, который обеспечивает автоматический деплой на удаленный сервер и отправку уведомлений в Telegram при каждом обновлении кода. Проект ранее был развернут и доступен по адресу http://51.250.25.224.


## Технологический стек
![Django-app workflow](https://github.com/Sergey-Ivch/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
[![Python](https://img.shields.io/badge/-Python-464646?style=flat&logo=Python&logoColor=56C0C0&color=008080)](https://www.python.org/)
[![Django](https://img.shields.io/badge/-Django-464646?style=flat&logo=Django&logoColor=56C0C0&color=008080)](https://www.djangoproject.com/)
[![Django REST Framework](https://img.shields.io/badge/-Django%20REST%20Framework-464646?style=flat&logo=Django%20REST%20Framework&logoColor=56C0C0&color=008080)](https://www.django-rest-framework.org/)
[![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-464646?style=flat&logo=PostgreSQL&logoColor=56C0C0&color=008080)](https://www.postgresql.org/)
[![JWT](https://img.shields.io/badge/-JWT-464646?style=flat&color=008080)](https://jwt.io/)
[![Nginx](https://img.shields.io/badge/-NGINX-464646?style=flat&logo=NGINX&logoColor=56C0C0&color=008080)](https://nginx.org/ru/)
[![gunicorn](https://img.shields.io/badge/-gunicorn-464646?style=flat&logo=gunicorn&logoColor=56C0C0&color=008080)](https://gunicorn.org/)
[![Docker](https://img.shields.io/badge/-Docker-464646?style=flat&logo=Docker&logoColor=56C0C0&color=008080)](https://www.docker.com/)
[![Docker-compose](https://img.shields.io/badge/-Docker%20compose-464646?style=flat&logo=Docker&logoColor=56C0C0&color=008080)](https://www.docker.com/)
[![Docker Hub](https://img.shields.io/badge/-Docker%20Hub-464646?style=flat&logo=Docker&logoColor=56C0C0&color=008080)](https://www.docker.com/products/docker-hub)
[![GitHub%20Actions](https://img.shields.io/badge/-GitHub%20Actions-464646?style=flat&logo=GitHub%20actions&logoColor=56C0C0&color=008080)](https://github.com/features/actions)
[![Yandex.Cloud](https://img.shields.io/badge/-Yandex.Cloud-464646?style=flat&logo=Yandex.Cloud&logoColor=56C0C0&color=008080)](https://cloud.yandex.ru/)


## Workflow
* build_and_push_to_docker_hub - Сборка и доставка докер-образов на Docker Hub
* deploy - Автоматический деплой проекта на боевой сервер. Выполняется копирование файлов из репозитория на сервер:
* send_message - Отправка уведомления в Telegram

### Подготовка для запуска workflow
Создайте и активируйте виртуальное окружение, обновите pip:
```
python3 -m venv venv
. venv/bin/activate
python3 -m pip install --upgrade pip
```
Запустите автотесты:
```
pytest
```
Отредактируйте файл `nginx/default.conf` и в строке `server_name` впишите IP виртуальной машины (сервера).

Скопируйте подготовленные файлы `docker-compose.yaml` и `nginx/default.conf` из директории проекта где находятся файлы на сервер:

Команды используются в BASH от имени администратора.
```
sudo scp docker-compose.yaml <username>@<host>/home/<username>/docker-compose.yaml
sudo mkdir nginx
sudo scp default.conf <username>@<host>/home/<username>/nginx/default.conf
```
Или добавьте их в ручную на удаленном сервере.

В репозитории на Гитхабе добавьте данные в `Settings - Secrets - Actions secrets`:
```
DOCKER_USERNAME - имя пользователя в DockerHub
DOCKER_PASSWORD - пароль пользователя в DockerHub
HOST - ip-адрес сервера
USER - пользователь
SSH_KEY - приватный ssh-ключ (публичный должен быть на сервере)
PASSPHRASE - кодовая фраза для ssh-ключа
DB_ENGINE - django.db.backends.postgresql
DB_NAME - postgres (по умолчанию)
POSTGRES_USER - postgres (по умолчанию)
POSTGRES_PASSWORD - postgres (по умолчанию)
DB_HOST - db
DB_PORT - 5432
SECRET_KEY - секретный ПРИВАТНЫЙ ключ приложения django ()
ALLOWED_HOSTS - список разрешённых адресов
TELEGRAM_TO - id своего телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN - токен бота (получить токен можно у @BotFather, /token, имя бота)
```
При внесении любых изменений в проект, после коммита и пуша
```
git add .
git commit -m "..."
git push
```
запускается набор блоков команд jobs (см. файл yamdb_workflow.yaml), т.к. команда `git push` является триггером workflow проекта.

## Как развернуть проект на сервере:
Установите соединение с сервером:
```
ssh username@server_address
```
Проверьте статус nginx:
```
sudo service nginx status
```
Если nginx запущен, остановите его:
```
sudo systemctl stop nginx
```
Установите Docker и Docker-compose:
```
sudo apt install docker.io
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
Проверьте корректность установки Docker-compose:
```
sudo  docker-compose --version
```
Создайте папку `nginx`:
```
mkdir nginx
```
### После успешного деплоя:
Соберите статические файлы (статику):
```
sudo docker-compose exec web python manage.py collectstatic --no-input
```
Примените миграции:
```
(опционально)sudo docker-compose exec web python manage.py makemigrations
sudo docker-compose exec web python manage.py migrate --noinput
```
Создайте суперпользователя:
```
sudo docker-compose exec web python manage.py createsuperuser
```

## Как развернуть проект локально:

Клонируйте репозиторий и перейдите в него в командной строке:
```
git clone https://github.com/Sergey-Ivch/Diplom.git
```
Создайте файл .env в папке infra/ командой `touch .env` и добавьте в него переменные окружения для работы с базой данных:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```
В папке проекта создайте образ:
```
docker build -t username/imagename:version backend/.
```
Соберите контейнеры:
```
docker-compose -f infra/docker-compose.yaml up -d --build
```
или пересоберите:
```
docker-compose up -d --build
```
Выполните миграции(Команды, начинающиеся с docker, выполняются в директории с файлом docker-compose.yaml.):
```
(опционально) docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate
```
Загрузка static:
```
docker-compose exec web python manage.py collectstatic --no-input
```
Создать сюперюзера:
```
docker-compose exec web python manage.py createsuperuser
```

### API YaMDb - эндпойнт на локальном сервере:
```json
http://localhost/admin/
http://localhost/recipes/
все эндпоинты в статической документации
```

### Генерация статической документации
```
pip install pyyaml
```
```
python3 manage.py generateschema > schema.yaml
```

### Документация API YaMDb
### Загрузите redoc.yaml на сайт
```
https://redocly.github.io/redoc/
```

## Авторы
[Ивченков Сергей](https://github.com/Sergey-Ivch)
