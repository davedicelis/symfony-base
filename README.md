# Repositorio Base de Symfony

Este repositorio contiene la configuración básica para ejecutar aplicaciones Symfony con base de datos MySQL

## Contenido
- Contenedor PHP-APACHE con PHP 8.5 (Composer 2, Symfony CLI, Xdebug 3.5, APCu)
- Contenedor MySQL con la versión 8.4 (LTS)
- Xdebug se ejecuta bajo demanda (`XDEBUG_TRIGGER`, `?XDEBUG_SESSION=1` o una extensión del navegador)
- Pensado para Symfony 7.4 (LTS, correcciones de seguridad hasta noviembre de 2029)

## Instrucciones
- `make build` para construir los contenedores
- `make start` para iniciar los contenedores
- `make stop` para detener los contenedores
- `make restart` para reiniciar los contenedores
- `make prepare` para instalar las dependencias con composer (una vez creado el proyecto)
- `make logs` para ver los logs de la aplicación
- `make ssh` para acceder por SSH al contenedor de la aplicación

## Crear y ejecutar la aplicación

> [!TIP]
> Reemplaza todas las apariciones de `symfony-app` (y el nombre de la base de datos `symfony_app`) en el proyecto por un nombre más significativo.
> Puedes usar la opción de buscar y reemplazar de tu IDE para hacerlo. No modifiques `symfony-cli` en el Dockerfile.

1. Construye e inicia los contenedores:
    ```shell
    make start
    ```
2. Accede por SSH al contenedor:
    ```shell
    make ssh
     ```
3. Crea un proyecto Symfony usando el CLI:
    ```shell
    symfony new --no-git --version=lts --dir project
    ```
4. Mueve todo el contenido de la carpeta `project` a la raíz del repositorio:
    ```shell
    mv project/{*,.*} . && rm -r project/
    ```
5. Añade el contenido del archivo `.gitignore` al de la raíz; debería quedar así:
    ```text
    .idea
    .vscode
    docker-compose.yml
    
    ###> symfony/framework-bundle ###
    /.env.local
    /.env.local.php
    /.env.*.local
    /config/secrets/prod/prod.decrypt.private.php
    /public/bundles/
    /var/
    /vendor/
    ###< symfony/framework-bundle ###
    ```
6. Una vez instalada tu aplicación Symfony, ve a http://localhost:1000
