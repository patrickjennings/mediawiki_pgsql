This is still in testing.

# Mediawiki / PGSQL
This allows for running a [Mediawiki](https://www.mediawiki.org/wiki/MediaWiki) service stack as a docker container or containers in swarm mode.

This stack uses the following:
* [php:7.2.7-fpm-alpine3.7](https://hub.docker.com/_/php/)
* [nginx:alpine](https://hub.docker.com/_/nginx/)
* [postgres:10.4-alpine](https://hub.docker.com/_/postgres/)


# Installation
In docker-compose.yml, comment out the volume links for LocalSettings.php and logo.png on first start.

```bash
docker-compose up
```

Navigate with your browser to the [service](http://localhost:8080) and complete the initial setup.

Once done, you will need to copy the LocalSettings.php in the container and reset the volume links in docker-compose.yml.


# References
* [Hermsi1337/full_php_dev_stack](https://github.com/Hermsi1337/docker-compose/tree/master/full_php_dev_stack)
* [kristophjunge/docker-mediawiki](https://github.com/kristophjunge/docker-mediawiki)
* [pastakhov/compose-mediawiki-ubuntu](https://github.com/pastakhov/compose-mediawiki-ubuntu)


# TODO
* Provide generic LocalSettings.php file.
    * Environment variables for setting database variables.
* Provide generic logo.png file.
* Docker Swarm support.
    * Setup a way to scale the PHP FPM workers.
        * Research how the overlay network load balances to the workers.
        * May require shared volume for supporting file uploads?
    * Setup a way to scale the Postgres database.
        * May require shared volume?
* Slim down the mediawiki service (currently 452.1 MB).
