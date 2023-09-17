# Docker_Start
Docker/ Django / Postgres / Redis
CONFIGURE NETWORK
docker network create my-network  # Создать сеть Docker с именем my-network

RUN REDIS
docker run -d --name redis --network my-network -p 6379:6379 redis  # Запустить контейнер Redis в сети my-network, прослушивающий порт 6379

RUN POSTGRESQL
docker run -d --name django_db --network my-network -p 5432:5432 -v C:\DOCKER\pgdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=lab -e POSTGRES_HOST_AUTH_METHOD=md5 postgres  # Запустить контейнер PostgreSQL в сети my-network, прослушивающий порт 5432, с настройками для базы данных lab и паролем postgres

RUN DJANGO
docker run -d --name django_app --network my-network -p 8000:8000 -it python bash  # Запустить контейнер Django в сети my-network, прослушивающий порт 8000, с интерактивной оболочкой Python

CONFIGURE DJANGO
docker exec -it django_app bash  # Выполнить команду внутри контейнера django_app с использованием интерактивной оболочки

mkdir app  # Создать каталог app

cd app  # Перейти в каталог app

pip install django  # Установить Django

apt update  # Обновить пакеты системы

apt install nano  # Установить текстовый редактор Nano

pip install psycopg2  # Установить драйвер PostgreSQL для Django

pip install redis  # Установить драйвер Redis для Django

ADD TO DJANGO/SETTINGS
ALLOWED_HOSTS = ['127.0.0.1']  # Разрешенные хосты

DATABASES = { 'default': { 'ENGINE': 'django.db.backends.postgresql',  # Настройки базы данных PostgreSQL
'NAME': 'lab', 'USER': 'postgres', 'PASSWORD': 'postgres', 'HOST': 'django_db', 'PORT': '5432', } }

CACHES = { 'default': { 'BACKEND': 'django.core.cache.backends.redis.RedisCache', 'LOCATION': 'redis://redis:6379' } }  # Настройки кэширования с использованием Redis

CACHE_ENABLED = True  # Включить кэширование

RUN DJANGO SERVER
python manage.py makemigrations  # Создать миграции базы данных

python manage.py migrate  # Применить миграции к базе данных

python manage.py runserver 0.0.0.0:8000  # Запустить сервер Django, прослушивающий все доступные интерфейсы на порту 8000
