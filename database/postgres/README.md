`pg_dump -U <user> -W -h <ip/host> -p <port/default-5432> <dbname> > ./path/to/dump.dump` - дамп определенной базы данных POSTGRES

`psql -U <user> -d <database> -h <ip/host> -p <port/default-5432> -f /path/to/backup.sql` - восстановление из бекапа(дампа)

`pg_dump -U <user> -d <database> --table=table1 --table=table2 --data-only > backup.sql` - в данном примере создается бекап БД `<your_database>` но для таблиц `<table1>` и `<table2>` не бекапится данные (только пустая таблица)
