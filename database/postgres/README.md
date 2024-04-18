`pg_dump -U <user> -W -h <ip/host> -p <port/default-5432> <dbname> > ./path/to/dump.dump` - дамп определенной базы данных POSTGRES

`psql -U <user> -d <database> -h <ip/host> -p <port/default-5432> -f /path/to/backup.sql` - восстановление из бекапа(дампа)

