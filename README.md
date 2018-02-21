# Quickstart: Compose and Django
https://docs.docker.com/compose/django/#connect-the-database

1. Create an empty directory
2. Create a file callled __Dockerfile__
3. Add the following:

```docker
FROM python:3
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/
```

5. Create a __requirements.txt__
6. Add the required software in the file:

```docker
Django==2.0.1
psycopg2
```

7. Create a file called docker-compose.yml
8.  Add the following configurartion to the file:

```docker
version: '3'

services:
  db:
    image: postgres
  web:
    build: .
    command: python3 manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```

#### Create a Django project

1. Change to the root of your project directory
2. Run:

    $ sudo docker-compose run web django-admin.py startproject tutorial .

3. ls -l

#### Connect the database

1. In your project directory, edit the __composeexample/settings.py__ file.
2. Replace the `DATABASES = ...` with the following:

```python
DATABASES = {
'default': {
    'ENGINE': 'django.db.backends.postgresql',
    'NAME': 'postgres',
    'USER': 'postgres',
    'HOST': 'db',
    'PORT': 5432,
}
}
```

On certain platforms (Windows 10), you might need to edit ALLOWED_HOSTS inside settings.py and add your Docker host name or IP address to the list. For demo purposes, you can set the value to:

```python
  ALLOWED_HOSTS = ['*']
```

3. Run the __docker-compose__ up command:

    $ docker-compose up (-d)

4. go to __http://localhost:8000__

5. Shut down the services and clean up by using either:
    1. __ctrl z__
    2. __docker-compose down__

open a bash session inside the running container

    $ docker exec -it c067b67b5ad1 /bin/bash


    docker-compose run web django-admin.py startproject tutorial .









Unsetting a virtual machine:
    $ docker-machine env -u
    $ & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env -u | Invoke-Expression
