# Mongo DB

- `mongo --username <user> --password <password>` подключение к БД под определенным пользователем
  - `--authenticationDatabase <database>` указать БД
  - `--host <ip/hostname>` указать ip/host
  - `--port <port>` указать порт (по умолчанию 27017)
- `show dbs` посмотреть пользователей в СУБД
- `use <database>` подключиться к определенной БД
  - `db.getUsers()` посмотреть пользователей (пользователи хранятся в таблицах у самих БД)
- `show collections` посмотреть таблицы (коллекции) в выбранной БД
- `mongodump --host=<ip/hostname> ...` команда создания бекапа, далее в зависимости от того как нужно сохранить бекап
  - `--db <database> --out /path/to/backup` сохранение бекапа по определенному пути (структура бекапа будет обычная директория с файлами json)
    - `mongorestore --host=<ip/hostname> --db <database> /path/to/backup/database/` восстановление БД из бекапа
  - `--gzip -d <database> --archive=/tmp/backup.gz` сохранение бекапа в заархивированном формате
    - `mongorestore --host=<ip=hostname> --gzip --archive=backup-db-habr.gz` восстановление БД из бекапа
