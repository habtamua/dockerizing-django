## Django Development With Docker Compose and Machine

Featuring:

- Docker v18.09.2
- Docker Compose v1.23.2
- Docker Machine v0.16.1
- Python 3.7.3

Blog post -> https://realpython.com/blog/python/django-development-with-docker-compose-and-machine/

### OS X Instructions

1. Start new machine - `docker-machine create -d virtualbox dev;`
1. Configure your shell to use the new machine environment - `eval $(docker-machine env dev)`
1. To view the currently running Machines - `docker-machine ls`
1. Build images - `docker-compose build`
1. Start services - `docker-compose up -d`
1. Create migrations - `docker-compose run web /usr/local/bin/python manage.py migrate`
1. Grab IP - `docker-machine ip dev` - and view in your browser


Here, the four services definition - web, nginx, postgres, and redis.

First, the web service is built via the instructions in the Dockerfile within the “web” directory - where the Python environment is setup, requirements are installed, and the Django application is fired up on port 8000. That port is then forwarded to port 80 on the host environment - e.g., the Docker Machine. This service also adds environment variables to the container that are defined in the .env file.
The nginx service is used for reverse proxy to forward requests either to Django or the static file directory.
Next, the postgres service is built from the the official PostgreSQL image from Docker Hub, which installs Postgres and runs the server on the default port 5432. Did you notice the data volume? This helps ensure that the data persists even if the Postgres container is deleted.
Likewise, the redis service uses the official Redis image to install, well, Redis and then the service is ran on port 6379.
