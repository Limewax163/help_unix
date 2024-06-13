<div align="center">

## [manage partition](https://github.com/Limewax163/help_unix/tree/main/manage_partition/README.md)

## [ssl](https://github.com/Limewax163/help_unix/tree/main/SSL/README.md)

## [msmtp](https://github.com/Limewax163/help_unix/tree/main/msmtp/README.md)

## [ufw](https://github.com/Limewax163/help_unix/tree/main/ufw/README.md)

## [database](https://github.com/Limewax163/help_unix/blob/main/database/README..md)

## [SSH](https://github.com/Limewax163/help_unix/blob/main/SSH/README.md)

## [make](https://github.com/Limewax163/help_unix/tree/main/make/README.md)

## [win](https://github.com/Limewax163/help_unix/tree/main/win/README.md)

## [logrotate](https://github.com/Limewax163/help_unix/blob/main/logrotate/README.md)
</div>

- `scp </path/to/file/file.txt> <user>@<host>:/</path/to/destination/>` - Копирование файлв через протокол ssh с одного хоста на другой
- `rsync -av </path/to/destination/folder> <user>@<host>:/</path/to/destination/>` - Копирование директории через собственный протокол (работает поверх TCP) с одного хоста на другой (флаг -a `archive` архивирует перед передачей, для того чтобы сохранить все свойства папок и файлов, флаг -v `verbose` для отображения на экран подробной информации о процессе копирования)
- `adduser <name user>` создать пользователя linux
- `usermod -aG <group> <user>` добавляет пользователя `<user>` в группу `<group>` ( -addGROUP )
- `usermod -md </new/home/dir> <user>` меняет домашнюю директорию пользователя (флаг -d) и переносит все ее содержимое вместе с доступами (флаг -m)
- `fail2ban-client status sshd` посмотреть список блокировок по ssh в fail2ban
- `fail2ban-client set sshd unbanip <ip чела>` разблокировать ip в fail2ban
- `lsof -nP -iTCP<port/or nothing to all> -sTCP:LISTEN` посмотреть список всех прослушиваемых портов
- `:w !sudo tee %` если вдруг запущен vim все отредактировано а доступа нет (сохраняет файл с правами sudo)
- `tcpdump -i any -n tcp port 10050` - интерактивно слушает все пакеты которые бегают по опредленному порту (в данном случае по 10050)
- `which <shell_service>` - команда которая показывает где находится исполняемый программой bin файл (например which docker вернет информацию о местоположении исполняемого файла docker - /usr/bin/docker)
- `resolvectl flush-caches` - сброс кэша DNS на сервере (старая команда: `systemd-resolve --flush-caches`)
- `smem -s swap` проверка используемой swap памяти процессами
- `source .env && echo $DB_PASSWORD` посмотреть как читается переменная из определенного файла

___

<details>
  <summary><b>du и NCDU</b></summary>

`sudo du -sch * .[!.]* | sort -rh | head` - для поиска и сортировки по размеру через du

1. `du` команда для вывода информации о размере файлов и директорий.
2. `-shc` * .[!.]* это опции du, которые указывают команде следующее:
- `s` (или --summarize) позволяет показать только общий размер для каждого аргумента (файл или директория)
- `h` (или --human-readable) выводит размер в удобном для чтения формате (например, KB, MB, GB)
- `c` (или --total) добавляет итоговую строку в конце вывода с общим размером всех файлов и директорий
- * - обрабатывает все файлы и директории в текущей директории.
- `.[!.]*` - обрабатывает скрытые файлы и директории (начинающиеся с точки), кроме . 
3. `|` - это символ "pipe", который перенаправляет вывод одной команды на ввод другой
4. `sort -rh` это команда для сортировки входных данных:
- `-r` (или --reverse) - сортирует в обратном порядке
- `-h` (или --human-numeric-sort) сортирует числа в удобном для чтения формате (например, KB, MB, GB)
5. `head` - это команда для вывода первых строк входных данных (по умолчанию первые 10 строк)

`ncdu /path/to/scan` сервис который сканирует файловую систему и показывает занятое место (визуальный буст du)

</details>

___

<details>
  <summary><b>Архиватор tar</b></summary>

- Флаги:
  - `-c` --create: Создать архив.
  - `-x` --extract, --get: Извлечь файлы из архива.
  - `-f` --file <архив>: Указать имя файла архива.
  - `-t` --list: Показать содержимое архива.
  - `-v` --verbose: Вывести подробный вывод.
  - `-z` --gzip: Использовать gzip для компрессии или декомпрессии архива.
  - `-j` --bzip2: Использовать bzip2 для компрессии или декомпрессии архива.
  - `-r` --append: Добавить файлы к существующему архиву.
  - `--delete:` Удалить файлы из архива.
  - `--strip-components <число>:` Удалить указанное количество компонент пути при извлечении файлов.
  - `--exclude=<шаблон>:` Исключить файлы, соответствующие шаблону.
  - `--wildcards:` Включить обработку шаблонов в именах файлов.

</details>

___

<details>
  <summary><b>RSYNC</b></summary>

`rsync -av --ignore-non-existing test2/ test1/`

> rsync в данной команде из директории test2 рекурсивно копирует все файлы и вставляет в директорию test1, причем всю остальную структуру директорий и каталогов он оставляет не тронутой. Условный файл находившийся в test2/path/to/FILE будет скопирован в директорию
> test1/path/to/FILE, а остальные файлы в этой директори останутся не тронутыми

- по флагам:

  - `-a` - это флаг для архивного режима, который включает рекурсивное копирование файлов и директорий, сохранение метаданных (включая права доступа, временные метки и т.д.), и также сохраняет ссылки и устройства.
  - `-v` - это флаг, который включает подробный вывод (verbose mode), позволяя увидеть информацию о копируемых файлах и процессе.
  - `--ignore-non-existing` - опция, которая позволяет игнорировать отсутствующие файлы в директории назначения (если в директории назначения есть файл test_file, а в директории с которой синхронизируются такого файла нет, то он не будет учтен в синхронизации)
  - `--ignore-existing` - опция, которая позволяет игнорировать существующие файлы в директории назначения (если в диретории назначения есть файл test_file И в директории с которой выполняется синхронизация этот файл есть, то он не будет учтен для синхронизации)
  - `--exclude=".env"` - опция, которая исключает копирование файлов (например с расширением  .env и т.д.)

</details>

___


<details>
  <summary><b>RSYNC</b></summary>

```
#!/bin/bash

PATH_BACKUP="$HOME/DB-BACKUP"

##################################################################
#Зашил железно конфигу от lk-develop.soglasie.ru
#PATH_PROJECT="/home/soglasie/www/lk-develop.soglasie.ru"

# Директория с файлом .env на выбор
#options=("lk-develop" "рядом со скриптом")
#echo "С какого стенда необходимо сделать бекап?:"
#select opt in "${options[@]}"
#do
#    case $opt in
#        "lk-develop")
#            PATH_PROJECT="$HOME/www/lk-develop.soglasie.ru"
#            break
#            ;;
#        "рядом со скриптом")
#            PATH_PROJECT="$PWD/.env"
#            break
#            ;;
#        *) echo "Некорректный выбор";;
#    esac
#done
##################################################################


ENV_PATH="$PATH_BACKUP/.env"


##################################################################
#Зашил железно файл contributing со списком таблиц из задачи DCM-3269
EXCLUDE_FILE="contributing"

#echo "Введите имя файла со списком таблиц"
#read EXCLUDE_FILE
#EXCLUDE_TABLE_DATA=()
# Проверяем наличие файла с названиями таблиц
#if [ -f "$PATH_BACKUP/$EXCLUDE_FILE" ]; then
#
#    # Считываем названия таблиц из файла в массив
#    while IFS= read -r table
#    do
#        EXCLUDE_TABLE_DATA+=("$table")
#    done < "$PATH_BACKUP/$EXCLUDE_FILE"
#
#else
#    echo "Поместите рядом со скриптом файл со списком таблиц которые следует бекапить без данных и укажите его в скрипте"
#    exit 1
#fi
##################################################################


# Название файла бекапа
BACKUP_FILE="$PATH_BACKUP/$EXCLUDE_FILE-$(date +'%d.%m.%Y').sql"

source "$ENV_PATH"

# 1й этап команды для выполнения в контейнере
# Подключение к базе данных, параметры для бекапа
COMMAND="pg_dump -U $DB_USERNAME -d $DB_DATABASE --clean --create --if-exists --no-owner -x --verbose"

# 2й этап команды для выполнения в контейнере
# Добавляем опцию --exclude-table-data для каждой таблицы из массива
for table in "${EXCLUDE_TABLE_DATA[@]}"
do
    COMMAND+=" --exclude-table-data 'lk.${table}*'"
done

#Переходим в каталог проекта
cd "$PATH_PROJECT"

# Исполняемая команда (включая $COMMAND который выполняется в контейнере)
docker exec -i "$COMPOSE_PROJECT_NAME"_postgres_1 /bin/bash -c "$COMMAND" > $BACKUP_FILE

echo "Создан бекап $BACKUP_FILE"

sleep 2

#echo "Производится редактирование файла базы данных для корректного последующего восстановления из бекапа"

sleep 5

# Изменение название базы с lktest на lk (для прода)
sed -i "s/lktest/lk/g" $BACKUP_FILE

# Поиск фразы "DROP DATABASE IF EXISTS" и сохранения ее в переменной
db_line=$(grep -n 'DROP DATABASE IF EXISTS' $BACKUP_FILE | head -n 1 | cut -d: -f1)

# Добавление трех строк перед найденной строкой
sed -i "${db_line}i\\
DROP DATABASE IF EXISTS _lk;\\
CREATE DATABASE _lk;\\
\\\connect _lk" $BACKUP_FILE
```
</details>
