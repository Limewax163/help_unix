`scp </path/to/file/file.txt> <user>@<host>:/</path/to/destination/>` - Копирование через портокол ssh с одного хоста на другой

Миграция таблиц в бд
mysql -p -u <user> <database> -> source /path/to/database_dump.sql

`adduser <name user>` создать пользователя linux

`usermod -aG <group> <user>` добавляет пользователя `<user>` в группу `<group>` ( -addGROUP )

`usermod -md </new/home/dir> <user>` меняет домашнюю директорию пользователя (ключ -d) и переносит все ее содержимое вместе с доступами (-m)

`fail2ban-client status sshd`

`sudo fail2ban-client set sshd unbanip <ip чела>`
