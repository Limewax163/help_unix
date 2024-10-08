### Про подключение и по администрированию

<details>
  <summary><code>psql -U user -d database -h ip/host -p port/default-5432</code> - подключение к БД</summary>

- Флаги:
  - `-U` - Пользователь
  - `-d` - База
  - `-W` - Запрос ввода пароля
  - `-h` - Хост
  - `-p` - Порт

</details>

`pg_lsclusters` - посмотреть состояния кластеров

`sudo pg_lscluster <version> <cluster> <action> [-- <pg_ctl options>]` - произвести какие-то манипуляции с кластером

> [!NOTE]
> Все конфиги по умолчанию находятся в `/etc/postgresql/<version>/main/`
> - pg_hba.conf - про подключения
> - postgresql.conf - главные настройки

<details>
  <summary>Немного по командам внутри СУБД</summary>

> соответственно если БД находится в контейнере можно передать в него через `docker exec -it ... bash` не заходя в контейнер

  - `\l` - посмотреть список баз данных
  - `\c <db_name>` - выбрать БД
  - `\dt` - посмотреть таблицы в выбранной БД
    - возможно потребуется указать схему в формате `\dt <schema.*>`
  - `\dt+ <schema>.<table_name>` - посмотреть подробную информацию по таблице
  - `\du` - посмотреть пользователей
  - `SELECT * FROM <schema>.<table_name>;` - посмотреть структуру таблицы и содержимое
  - `CREATE USER <username> WITH PASSWORD '<password>';` - создать пользователя с паролем
  - `CREATE DATABASE <db_name>;` - создать БД
  - `GRANT ALL PRIVILEGES ON DATABASE database_name TO username;` - добавить права на таблицу пользователю (все)
  - `ALTER ROLE <user> <ROLE>;` - Добавить роль для пользователя (чтобы отнять `ALTER ROLE <user> NO<ROLE>;`)
    - `SUPERUSER`
    - `CREATEDB`
    - `CREATEROLE`
    - `LOGIN`
    - `...`
  
</details>

___

### Про бекапы

`psql -U user -d database -h ip/host -p port/default-5432 -f /path/to/backup.sql` - восстановление из бекапа(дампа)

`pg_dump -U <user> -d <database> > ./path/to/backup.sql` - дамп определенной базы данных

`pg_dump -U <user> -d <database> --exclude-table-data=public.table1 --exclude-table-data=public.table2 > backup.sql` - создается бекап БД `<database>` но для таблиц `<table1>` и `<table2>` не бекапится данные (только пустая таблица)

>[!NOTE]
>Если не указывать схему таблицы, то автоматически pgsq выберет схему `<public>` в примерах выше схема по умолчанию указана явно `--exclude-table-data=public.table1`
>
>Схема указывается в формате `<schema>.<table_name>`
