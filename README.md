# mediawiki-docker
Mediawiki in a docker-compose pair of containers

## Starting the service
Just run
> docker-compose up

If this is the first run with a new set of containers (especialy the database container) you will need to restore the database

## Restoring the database
The initdb.sql file contains the initial database created by the mediawiki installer and matches the LocalSetting.php in this repo

To restore this database you will need to have already run
> docker-compose up

to start the containers. At this point if you try to browse to localhost:8000 you will get a PHP exception, you now need to restore the database

Now run
> docker ps | grep mediawiki-docker_mysql

This will give you the container running the database for this project, the last peice is the container name eg
>5272d00c047a mariadb "docker-entrypoint.s…"   14 hours ago Up 4 minutes  3306/tcp   mediawiki-docker_mysql-database_1

In this case the container name is "mediawiki-docker_mysql-database_1". You will also need the database details from  compose/mysql.env

Now to restore the database run
>cat initdb.sql | docker exec -i [container name] /usr/bin/mysql -u [mysql user] --password=[password] [wiki database]

for the example above the command would be
>cat initdb.sql | docker exec -i mediawiki-docker_mysql-database_1 /usr/bin/mysql -u wikiuser --password=example my_wiki

## Backing up the database
If you want to backup the database, follow the steps above to get the container name then run

>docker exec [container name] /usr/bin/mysqldump -u [mysql user] --password=[mysqp password] [wiki database] > backup.sql

For the example above the command would be
>docker exec mediawiki-demo_mysql-database_1 /usr/bin/mysqldump -u wikiuser --password=example my_wiki > backup.sql
