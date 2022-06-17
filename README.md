This is still in testing.

# Mediawiki / PGSQL
This allows for running a [Mediawiki](https://www.mediawiki.org/wiki/MediaWiki) service stack as a docker container or containers in swarm mode.

This stack uses the following:
* [php:7.2.7-fpm-alpine3.7](https://hub.docker.com/_/php/)
* [nginx:alpine](https://hub.docker.com/_/nginx/)
* [postgres:10.4-alpine](https://hub.docker.com/_/postgres/)


# Installation
In docker-compose.yml, comment out the volume link for LocalSettings.php on first start.

```bash
docker-compose up
```

Navigate with your browser to http://localhost/mw-config/index.php and complete the initial setup.

For the database setup, select PostgreSQL with the following attributes:

| Database Attribute | Value |
|---|---|
| Database host | mediawikidb |
| Database port | 5432 |
| Database name | wikidb |
| Schema for MediaWiki | mediawiki |
| Database username | wikiuser |
| Database password | wikipass |

These are configurable through the environment variables in the docker-compose.yml

Once done, you will need to copy the LocalSettings.php and reset the volume link in the docker-compose.yml.

# Restore DB From SQL
You can seed the postgres database from an existing mediawiki database with the following command:

```bash
docker exec -i $(docker-compose ps -q mediawikidb) psql -Uwikiuser wikidb < backup.sql
```


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
