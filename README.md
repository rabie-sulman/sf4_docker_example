# how to create a Symfony4 within a Docker environment:
- after downloading, 
  ```bash
    > cd Docker
    > cp .env.dist .env
  ```
- change the APP_NAME env variable in the `.env` file to whatever you want your application to be called. Also, change the DB credentials and timezone if you wish
- start the docker container:
  ```bash
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
- browse your nginx IP (native Docker? `0.0.0.0`) and you'll see the Symfony welcome page
 
## notes
- the nginx config maps to the `public` folder as root folder as the new SF4 now changed from web
- php is build based on `php:7.1` and added on that is the `composer`, `pdo`, `xdebug` and other libraries (see `Docker/php/Dockerfile`)

