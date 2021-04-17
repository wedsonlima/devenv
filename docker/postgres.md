# PostgreSQL with docker

  You can use a different postgres version for each project you have.

## Create a new volume for each new PostgreSQL version

```console
docker volume create pgdata13
```

## Create a container using the volume

  If you want to be able to use other versions of postgres and access it with `psql` from `localhost` you need to link a different `$PORT` to each version.

  I like this pattern:

  | version    | port |
  |---------   |------|
  | postgres96 | 5496 |
  | postgres12 | 5412 |
  | postgres13 | 5413 |


```console
docker run --name postgres13 -e POSTGRES_PASSWORD=123456 -d -p 5413:5432 -v pgdata13:/var/lib/postgresql/data postgres:13
```

## Create a DB

```console
docker exec -i postgres13 psql -U postgres -c "CREATE DATABASE <db-name> WITH OWNER=postgres;"
```

## Start the container

```console
docker start postgres13
```

## Login

```console
docker exec -it postgres13 psql -U postgres
```

```console
psql -U postgres -p 5413 -h localhost
```

## Restore a dump

```console
docker exec -i postgres13 psql -U postgres -d <dbname> -f dump.sql
```
