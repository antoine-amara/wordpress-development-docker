# Wordpress development environment with docker.

A clean development environment for wordpress projects into docker containers. The installation of wordpress and his dependencies (plugins and themes) is managed with composer. The docker environment is managed with docker-compose.

**note**: the docker-compose and docker containers of this repo are not **production ready**, please use them **only** for development.

## File architecture

I define a file architecture to be able to easily manage content with composer and git. I take this from blogs article which was written by Gilbert Pellegrom, you can find the part 1 (and link to the other parts) [here](https://deliciousbrains.com/storing-wordpress-in-git/)

```shell
|
|-- public/
|   |-- wp-content/
|   |-- wp/
|   |-- .htaccess
|   |-- index.php
|   `-- wp-config.php
|
|-- .gitignore
|-- composer.json
`-- local-config.sample.php
```

* public: it is the exposed folder (by the web server).
* wp-content: the content of the cms like plugins and themes.
* wp: contain cms core.
* wp-config.php: basic configuration file . It loads local-config.php or production-config.php which contains critical configurations for the cms and database. This file is not into the public folder for a security reason.

## Installation and first run
We have to use the docker-compose api to install dependencies. After that, we will add a configuration file to connect wordpress instance to the database and we will go on the browser to access to wordpress.

First, we have to call composer to install all components:
```shell
docker-compose run --rm wordpress composer install
```

Next, copy local-config.sample.php to local-config.php and replace line 1 to 4 by:
```php
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wordpress' );
define( 'DB_PASSWORD', 'wordpress' );
define( 'DB_HOST', 'db' );
```
These lines are usefull to connect our wordpress instance to the container database.

Now run the environment:
```shell
docker-compose up -d
```

Finally, you can open your browser and go to 'http://localhost:8000' to run the wordpress installation and access to your new wordpress instance.

## Run and manage the environment
This configuration work with docker-compose, you have to manage it with docker-compose api. In this README, I will give you some main command to use, if you want to have more please see the [docker documentation](https://docs.docker.com/compose/).

First, to run the application run:
```shell
docker-compose up -d
```

If you have to update the docker image for wordpress (the Dockerfile is into `Docker/php/`)m you have to rebuild your image for docker-compose with:
```shell
docker-compose build
```

If you want to stop all containers:
```shell
docker-compose rm
```

Finally, if you don't need the environment anymore you can destroy it and delete all component (containers, networks, and volumes):

**WARNING** this command will remove volumes which contain the database content so this command will overwrite your wordpress data, users, etc...

```shell
docker-compose down -v
```
