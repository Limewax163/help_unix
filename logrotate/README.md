```
cat /etc/logrotate.conf
# see "man logrotate" for details

# global options do not affect preceding include directives

# Настройка обновления daily/weekly/monthly
weekly

# use the adm group by default, since this is the owning group
# of /var/log/syslog.
logs su root adm

# Указывает, сколько старых логов нужно хранить (в параметрах передается количество)
rotate 3

# Создание пустого файла лога после перемещения старого (применяется старое название лога)
create

# К старым логам после ротации добавляется суффикс. В данном случае мы задаем формат добавления - dateformat-%Y-%m-%d
dateext dateformat-%Y-%m-%d

# После ротации старый файл сжимается gzip. Не сжимает последний и предпоследний журналы (если файлы могут быть заняты процессом)
compress
delaycompress

# packages drop log rotation information into this directory
include /etc/logrotate.d

# system-specific logs may also be configured here.

# Директория в которую перемещаются (после ротации) старые файлы
olddir /var/log/old_logs

####################################################################################################
#### Для проверки настроек можно воспользоваться командой logrotate -d /etc/logrotate.d/bootlog ####
####################################################################################################
```
