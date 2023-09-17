# **Docker_Start**
**Docker/** **Django** / **Postgres** / **Redis**

### **CONFIGURE NETWORK**
  * docker network create my-network

### **RUN REDIS**
  * docker run -d --name redis --network my-network -p 6379:6379 redis

### **RUN POSTGRESQL**
  * docker run -d --name django_db --network my-network -p 5432:5432 -v C:\DOCKER\pgdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=lab -e POSTGRES_HOST_AUTH_METHOD=md5 postgres

### **RUN DJANGO**
  * docker run -d --name django_app --network my-network -p 8000:8000 -it python bash

### **CONFIGURE DJANGO**
  * docker exec -it django_app bash
  * mkdir app
  * cd app
  * pip install django
  * apt update
  * apt install nano
  * pip install psycopg2
  * pip install redis

### **ADD TO DJANGO/SETTINGS**
  * ALLOWED_HOSTS = ['127.0.0.1']

  * DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'lab',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'django_db',
        'PORT': '5432',
    }
}

  * CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.redis.RedisCache',
        'LOCATION': 'redis://redis:6379',
    }
}

  * CACHE_ENABLED = True

### **RUN DJANGO SERVER**
  * python manage.py makemigrations
  * python manage.py migrate
  * python manage.py runserver 0.0.0.0:8000
