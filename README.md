# Description
A Docker Compose workflow for local Laravel development.
# Containers

- NGINX
> Webserver with PDO extension
- PHP-FPM
> PHP FastCGI Process Manager
- MySQL
> Database
- Artisan
> **Usage**: docker-compose run --rm artisan [YOUR ARTISAM COMMAND]
- Composer
> **Usage**: docker-compose run --rm composer [YOUR COMPOSER COMMAND]
- NPM
> **Usage**: docker-compose run --rm npm [YOUR NPM COMMAND]
- Git 
> **Usage:** docker-compose run --rm git [YOUR_GIT_COMMAND]

# Deploy a Laravel project
## Change values
Change the default values in **docker-compose.yml** file.
> For example change the NGINX port and **MySQL environment values**.


## Start containers
```sh
docker-compose up -d --build
```


## Create project files
Copy files manually

```sh
cp -aR [FROM_DIRECTORY] src/
```
Or Clone a Git repository
```sh
docker-compose run --rm git clone {YOUR_REPOSITORY_URL} .
```
> **Note:** Don't forget the dot in the end of the command

Or Create a new project via Composer
```sh
docker-compose run --rm create-project --prefer-dist laravel/laravel .
```

> **Note:** Don't forget the dot in the end of the command

## Set permissions
```sh
docker-compose exec nginx chmod -R 777 /var/www/html/storage
docker-compose exec nginx chmod -R 775 /var/www/html/bootstrap
```

## Create the .env file
```sh
cp src/.env.example src/.env
```

> **Note:** Don't forget to change the environment values
> **Note:** The **DB_HOST** must be equal with mysql



## Artisan commands
Generate Application key
```sh
docker-compose run --rm artisan key:generate
```
Running database migrations
```sh
docker-compose run --rm artisan migrate
```
Running database seeders
```sh
docker-compose run --rm artisan db:seed
```

# Troubleshooting
## NGINX replies with 500 error code after deploy
Just create your **.env** file in your project source folder (**src**)

## Permission denied on files
```sh
docker-compose exec nginx chmod -R 777 /var/www/html/storage
docker-compose exec nginx chmod -R 775 /var/www/html/bootstrap
```

## MySQL access denied for user (using password yes)
- Your MySQL password not matched
- Or you need to delete the MySQL files, stored in the **mysql-data** folder
```sh
docker-compose stop mysql
rm -rf mysql-data/*
docker-compose start mysql
docker-compose run --rm artisan migrate
docker-compose run --rm artisan db:seed
```