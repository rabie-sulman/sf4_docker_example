# how to create a Symfony4 within a Docker environment:
After downloading:
  ```bash
    > cd Docker
    > cp .env.dist .env
  ```
- change the APP_NAME env variable in the `.env` file to whatever you want your application to be called. Also, change the DB credentials and timezone if you wish. 

#### With Makefile:
- You can initialise the project with a tools container and a Makefile command by: 
  ```bash
    > cd Docker
    > # create a Symfony project from symfony/skeleton
    > make init-symfony
    > # if you want to initialise with Encore, this will run the above command if the application directory doesn't exists (becasue Symfony is required)
    > make init-symfony-encore
  ```
- browse your localhost and you'll see the Symfony welcome page
#### From within the containers:
- start the docker container:
  ```bash
  	> # note that here we are using the dev containers rather than the tools one
    > docker-compose up --build -d
  ```
- login to the php container:
  ```bash
    > docker exec -it docker_php_1 bash
  ```
- install the Symfony4 project with composer as you'd normally do (locally in the folder): 
  ```bash
    > composer create-project symfony/skeleton .
  ```
- If you need to initialise the Encore, you will need to use the tools container:
  ```bash
  	> docker-compose -f docker-compose-dev.yml up --build -d
  	> # login to the container, note that the files are mapped to /tmp
  	> docker exec -it docker_tools_1 bash
  	> # following the Guidlines on https://symfony.com/doc/current/frontend/encore/installation.html
    > yarn add @symfony/webpack-encore --dev
    > composer require encore
    > yarn install
  ```
- browse your localhost and you'll see the Symfony welcome page
 
## notes
- it is recommended to make use of the Makefile
- the tools container is based on php-fpm 7.1 and also has node and yarn installed globally. It also uses a non-root user `tools` for permissions for the `composer`, `npm` and `yarn` commands.
- the tools container is only for project initialisation purposes
- the nginx config maps to the `public` folder as root folder as the new SF4 now changed from web
- php is build based on `php:7.1` and added on that is the `composer`, `pdo`, `xdebug` and other libraries (see `Docker/php/Dockerfile`)

