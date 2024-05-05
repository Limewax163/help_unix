<div align="center">

## [manage partition](https://github.com/Limewax163/help_unix/tree/main/manage_partition/README.md)

## [ssl](https://github.com/Limewax163/help_unix/tree/main/SSL/README.md)

## [msmtp](https://github.com/Limewax163/help_unix/tree/main/msmtp/README.md)

## [ufw](https://github.com/Limewax163/help_unix/tree/main/ufw/README.md)

## [database](https://github.com/Limewax163/help_unix/blob/main/database/README..md)

## [SSH](https://github.com/Limewax163/help_unix/blob/main/SSH/README.md) ##
</div>

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

`:w !sudo tee %` если вдруг запущен vim все отредактировано а доступа нет (сохраняет файл с правами sudo)

`tcpdump -i any -n tcp port 10050` - интерактивно слушает все пакеты которые бегают по опредленному порту (в данном случае по 10050)

`which <shell_service>` - команда которая показывает где находится исполняемый программой bin файл (например which docker вернет информацию о местоположении исполняемого файла docker - /usr/bin/docker)



`docker ps -s --format "table {{.Names}}\t{{.Size}}" | sort -k2 -h -r` покажет сколько занимают памяти контейнеры с выводом по шаблону только имя контейнера и занятую память с сортировкой по убыванию занятой памяти

`docker system df` покажет сколько памяти используют все контейнеры в системе

Вы можете уничтожить все свои контейнеры и изображения с помощью `docker rm -f $(docker ps -aq) && docker rmi -f $(docker images -q)`

`ncdu` сервис который сканирует файловую систему и показывает занятое место


<details>
  <summary><code>sudo du -sch * .[!.]* | sort -rh | head</code> - для поиска и сортировки по размеру через du</summary>

1. du - это команда для вывода информации о размере файлов и директорий.

2. -sch * .[!.]* - это опции du, которые указывают команде следующее:

- s (или --summarize) - позволяет показать только общий размер для каждого аргумента (файл или директория).

- c (или --total) - добавляет итоговую строку в конце вывода с общим размером всех файлов и директорий.

- h (или --human-readable) - выводит размер в удобном для чтения формате (например, KB, MB, GB).

После опций du указаны аргументы:

- * - обрабатывает все файлы и директории в текущей директории.

- .[!.]* - обрабатывает скрытые файлы и директории (начинающиеся с точки), кроме . и ...

3. | - это символ "pipe", который перенаправляет вывод одной команды на ввод другой.

4. sort -rh - это команда для сортировки входных данных:

- sort - сортирует входные данные.

- -r (или --reverse) - сортирует в обратном порядке.

- -h (или --human-numeric-sort) - сортирует числа в удобном для чтения формате (например, KB, MB, GB).

5. head - это команда для вывода первых строк входных данных (по умолчанию первые 10 строк).

</details>

`smem -s swap` - Проверка используемой swap памяти процессами

`source .env && echo $DB_PASSWORD` - так можно посмотреть как читается переменная из определенного файла

<details>
  <summary><code>rsync -av --ignore-non-existing test2/ test1/</code> - для более детального копирования файлов</summary>
__

> rsync в данной команде из директории test2 рекурсивно копирует все файлы и вставляет в директорию test1, причем всю остальную структуру директорий и каталогов он оставляет не тронутой. Условный файл находившийся в test2/path/to/FILE будет скопирован в директорию
> test1/path/to/FILE, а остальные файлы в этой директори останутся не тронутыми

- по флагам:

  - `-a` - это флаг для архивного режима, который включает рекурсивное копирование файлов и директорий, сохранение метаданных (включая права доступа, временные метки и т.д.), и также сохраняет ссылки и устройства.
  - `-v` - это флаг, который включает подробный вывод (verbose mode), позволяя увидеть информацию о копируемых файлах и процессе.
  - `--ignore-non-existing` - опция, которая позволяет игнорировать отсутствующие файлы в директории назначения (если в директории назначения есть файл test_file, а в директории с которой синхронизируются такого файла нет, то он не будет учтен в синхронизации)
  - `--ignore-existing` - опция, которая позволяет игнорировать существующие файлы в директории назначения (если в диретории назначения есть файл test_file И в директории с которой выполняется синхронизация этот файл есть, то он не будет учтен для синхронизации)
  - `--exclude=".env"` - опция, которая исключает копирование файлов (например с расширением  .env и т.д.)

</details>

> [!NOTE]
> Для автоматизации процессов можно использовать make. Создается файл `<Makefile>`.
> Пример и наглядное понимания построения структуры файла [`MAKEFILE`](https://github.com/Limewax163/k8s/blob/main/Makefile.md)
> 
> Makefile не нужны права на исполнение

### Архиватор tar

<details>
  <summary>Вызывается командой<code>tar</code></summary>

Флаги:
___
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


