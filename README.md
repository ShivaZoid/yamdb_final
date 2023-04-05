[![Yamdb_final workflow](https://github.com/ShivaZoid/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/ShivaZoid/yamdb_final/actions)

# Для Ревью: ip сервера < 130.193.40.223 >, login: <bzm>, password: <bzm>

# API_YAMDB

### REST API проект для сервиса YaMDb — сбор отзывов пользователей на произведения.

## Описание
Проект YaMDb собирает отзывы пользователей на произведения. Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку. Произведения делятся на категории, такие как «Книги», «Фильмы», «Музыка». Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Настроика для приложения Continuous Integration и Continuous Deployment, реализация:

- Автоматический запуск тестов.
- Обновление образов на Docker Hub.
- Автоматический деплой на боевой сервер при пуше в главную ветку main.
- Отправка оповещение об успешном завершении процесса при помощи телеграмм бота .

### Как запустить проект:
Клонируем репозиторий и переходим в него:
~~~
cd infra_sp2
cd api_yamdb
~~~

Создаем и активируем виртуальное окружение:
~~~
python3 -m venv venv
source /venv/bin/activate (source /venv/Scripts/activate - для Windows)
python -m pip install --upgrade pip
~~~

Ставим зависимости из requirements.txt:
~~~
pip install -r requirements.txt
~~~

Добавьте файл (.env) , расположенный по пути infra/.env
~~~
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
~~~

Добавьте в Secrets GitHub Actions переменные окружения для работы базы данных:
~~~
HOST - ip сервера
USER - login
SSH_KEY - cat ~/.ssh/id_rsa

DOCKER HUB:
DOCKER_USERNAME 
DOCKER_PASSWORD

PASSPHRASE
DB_ENGINE
DB_NAME
POSTGRES_USER
POSTGRES_PASSWORD
DB_HOST
DB_PORT

TELEGRAM_TO - ваш telegram_id
TELEGRAM_TOKEN - токен бота
~~~

## На сервере:

### Подготовьте сервер

Войдите на свой удаленный сервер

Остановите службу nginx:
~~~
 sudo systemctl stop nginx
~~~

Установите docker:
~~~
sudo apt install docker.io
~~~

Установите docker-compose:
~~~
sudo curl -SL https://github.com/docker/compose/releases/download/v2.17.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
~~~

Затем необходимо задать правильные разрешения, чтобы сделать команду docker-compose исполняемой:
~~~
sudo chmod +x /usr/local/bin/docker-compose
~~~

Чтобы проверить успешность установки, запустите следующую команду:
~~~
sudo docker-compose --version
~~~

Скопируйте файлы docker-compose.yaml и nginx/default.conf из вашего проекта на сервер в home/<ваш_username>/docker-compose.yaml и home/<ваш_username>/nginx/default.conf соответственно.

Выполняем миграции:
~~~
docker-compose exec web python manage.py migrate
~~~

Создаем суперпользователя:
~~~
docker-compose exec web python manage.py createsuperuser
~~~

Србираем статику:
~~~
docker-compose exec web python manage.py collectstatic --no-input
~~~

Создаем дамп базы данных (резервную копию):
~~~
docker-compose exec web python manage.py dumpdata > dumpPostrgeSQL.json
~~~

---
Документация API YaMDb доступна по эндпойнту: http:// < ip сервера > /redoc/

