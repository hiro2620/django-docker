# django-docker

docker-compose.yml and other files to launch develop environment for django with Nginx and PostgreSQL

## Usage

The instructions below are for Linux. If you're using a Mac or Windows, please read accordingly. 

STEP 1
Clone files. 
```bash
$ cd dir-to-work
$ git clone https://github.com/hiro2620/django-docker.git
$ cd django-docker
```
*`alias.sh` is just for running long comannd after django setup has finished,
and never used while setting up environment.

STEP 2
Build Docker containers.
It takes a few minuits, depending on yout internet speed. 
```bash
$ docker-compose up
```
If you want to run it on the background, run
```bash
$ docker-compose up -d
```
instead.
When you get messeages like this,
```
db_1      | 2020-05-20 05:39:41.098 UTC [1] LOG:  database system is ready to accept connections
```
, press `Ctrl+C` to stop containers and go to the next step.

STEP 3
Start Django project.
run
```bash
$ docker-compose run --rm python django-admin startproject app .
```
Be careful not to forget '.' at the end.

After that you can see django files in `./django-root`

Then, access `http://localhost:8010` on browser to check if Django project works fine.

![screenshot](https://user-images.githubusercontent.com/56952494/82412411-913e7d00-9aae-11ea-82b9-5c84bfec8edc.png)


STEP 4
Allow django to connect PostgreSQL.

Generated files in `./django-root/` is unwritable, so run
```bash
$ sudo chmod -R 777 django-root
```
to edit them.

then, open `./django-root/app/settings.py`
and replace
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```
with
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'password',
        'HOST': 'db',
        'PORT': '5432',
    }
}
```

and run following comannds to migrate database.
```
docker-compose run --rm python python3 manage.py makemigrations
docker-compose run --rm python python3 manage.py migrate
```

after migratin, run `collectstatic`(below) to apply css.
```bash
docker-compose run --rm python python3 manage.py collectstatic
```

Then, access `http://localhost:8010` again.
If it still works fine, Django connected Postgre SQL successfully,
and all step has been done.

If you want to create superuser, run
```bash
docker-compose run --rm python python3 manage.py createsuperuser
```


## NOTE

How to change port to access Django page?
  - Edit line line 7 in `docker-compose.yml`
  ```
  ports:
  - "8010:8000"
  ```
  this `8010` is the port that is used to access, so please edit it.
  
How to start app or collect static in django?
  - Run following comannd on console on host PC.
  ```bash
  docker-compose run --rm python python3 manage.py startapp # To run startapp
  docker-compose run --rm python python3 manage.py collectstatic #To run collectstatic
  ```
  This is also noted on `alias.sh` on the same directory to `docker-compose.yml`

How to stop containers started by `$ docker-compose up -d` ?
  - Run
  ```bash
  $ docker-compose down
  ```
  
## Licence

[MIT](https://github.com/hiro2620/django-docker/blob/master/LICENCE)

## Author

[hiro2620](https://github.com/hiro2620)

That's all, thanks:)
