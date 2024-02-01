## [manage partition](https://github.com/Limewax163/help_unix/tree/main/manage_partition)

## [ssl](https://github.com/Limewax163/help_unix/tree/main/SSL)

## [msmtp](https://github.com/Limewax163/help_unix/tree/main/msmtp)

## [ufw](https://github.com/Limewax163/help_unix/tree/main/ufw)

`scp </path/to/file/file.txt> <user>@<host>:/</path/to/destination/>` - Копирование файлв через протокол ssh с одного хоста на другой

`rsync -av </path/to/destination/folder> <user>@<host>:/</path/to/destination/>` - Копирование директории через собственный протокол (работает поверх TCP) с одного хоста на другой (флаг -a `archive` архивирует перед передачей, для того чтобы сохранить все свойства папок и файлов, флаг -v `verbose` для отображения на экран подробной информации о процессе копирования)

Миграция таблиц в бд
mysql -p -u <user> <database> -> source /path/to/database_dump.sql

`adduser <name user>` создать пользователя linux

`usermod -aG <group> <user>` добавляет пользователя `<user>` в группу `<group>` ( -addGROUP )

`usermod -md </new/home/dir> <user>` меняет домашнюю директорию пользователя (флаг -d) и переносит все ее содержимое вместе с доступами (флаг -m)

`fail2ban-client status sshd` посмотреть список блокировок по ssh в fail2ban

`fail2ban-client set sshd unbanip <ip чела>` разблокировать ip в fail2ban

`lsof -nP -iTCP -sTCP:LISTEN` посмотреть список всех прослушиваемых портов
