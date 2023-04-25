# Laravel Docker Dev Env (April'23)
A little docker environment for Laravel development.  

It uses the following docker images, paired with their names:
   - `webserver`: Nginx (Alpine)  
   - `db` : MariaDB  
   - `app`: PHP v8.2  

Structure of the Development environment:
   - 📁 `mysql` ➡️ db engine files
   - 📁 `nginx` ➡️ NGINX default configuration
   - 📁 `src`   ➡️ Laravel source code

## How to install
After cloning this repository and **BEFORE** composing it up:

> ⚠️ Be sure to have `docker` and `docker compose` installed. Here you have the
reference to get them installed in your system:  
https://docs.docker.com/compose/install/

> ⚠️ Also, check that you have `composer` installed in your system. Here you have a link to 
the documentation to get such PHP package manager in your system:  
https://getcomposer.org/doc/00-intro.md#globally

**First of all** , you would need to install the Laravel desired version source code:
```php
composer create-project laravel/laravel ./src/
```

**Then**, customize the database configuration within the ./src/.env. Remember to set the `DB_HOST` to the
database docker name, in this case `db`, and customize the database name, the username and the password parameters:
```php
DB_HOST=db
DB_PORT=3306
DB_DATABASE=<your_database_name>
DB_USERNAME=<database_username>
DB_PASSWORD=<username_password>
```

**After that**, set your `$user` (result of ```id -u```) and `$uid` (result of running ```id -g```) within the `docker-compose.yml`: 
```yml
  [...]
  app:
    build:
      args:
        user: $user
        uid: $uid
      context: ./
   [...]
```
Then, you can just use the docker compose set of commands:

   - FIRST TIME: build your docker compose setup with `docker-compose up --build`
   - Wake-up dockers: `docker-compose up -d`
   - Shutdown them: `docker-compose down`
   - Build `app`: `docker-compose build app`
   - Check status: `docker-compose ps`
   - Execute commands: `docker-compose exec app <command>`

> 💡 Keep in mind that the database information would be within the `./src/.env` file, which parameters
are used in the `docker-compose.yml` file in order to connect and create the database instance.
  
<br/>

#### ⏩ Once it is up and working, you should be able to load the Laravel site using the following URL:  
http://localhost:8888
<br/>
<br/>
  
***
### Some useful commands:
   - Migrations:  
   ```bash
   docker-compose exec app php artisan migrate
   ```

   - New model/controller/resource:
   ```bash
   docker-compose exec app php artisan make:[model/controller/resource] <options>
   ```

## Credits
Partly based in this article from DigitalOcean:  
https://www.digitalocean.com/community/tutorials/how-to-install-and-set-up-laravel-with-docker-compose-on-ubuntu-22-04
