<div align="center">

# УПРАВЛЕНИЕ РАЗДЕЛАМИ

</div>
  
`lsblk` Для просмотра доступных дисков\разделов\точек монтирования (чтобы проверить дополнительно UUID\тип файловой системы и ряда других параметров используется флаг -f)

`fdisk /dev/sd<X>` fdisk утилита для работы с дисками (создание\удаление). Данная команда запускает эту утилиту для работы с диском `/dev/sd<X>` где `<X>` буква диска

`sudo mkfs.ext4 /dev/sd<X><n>` - Форматировать раздел `n` диска `X` в ext4

<details>
  <summary>FDISK COMMAND</summary>

- l: Выводит список всех дисков и их разделов.
- p: Показать таблицу разделов для выбранного диска.
- m: Показать справку для утилиты fdisk.
- u: Показывает размеры разделов в секторах, а не в байтах.
- s: Выводит размер выбранного раздела в килобайтах.
- n: Создает новый раздел.
- t: Изменяет тип выбранного раздела.
- d: Удаляет выбранный раздел.
- v: Выводит подробную информацию о диске и разделах.
- h: Выводит справку о доступных ключах командной строки.
- w: Сохранить изменения и выйти из утилиты fdisk.
- q: Выйти из утилиты fdisk без сохранения изменений.

</details>

<details>
  <summary>SWAP</summary>

> Хорошим тоном будет - создать отдельный раздел под SWAP размером либо RAM x2, либо 1к1, но кто я такой чтобы говорить как нужно делать

- `free -h` - посмотреть информацию о swap/ram
- `swapon --show` - информация по текущему swap
- `cat /proc/sys/vm/swappiness` - swappiness значение эквивалентно которому будет использоваться swap относительно ОЗУ
  - `sudo sysctl vm.swappiness=60` - меняет значение, либо значение можно поменять в файле конфигурации:
    - Значение прописывается в конфиге `/etc/sysctl.conf` конфиг `vm.swappiness=60` значение по умолчанию 60
    - Применить изменения конфигурации `/etc/sysctl.conf` можно командой `sudo sysctl -p`

<details>
  <summary>Создание SWAP целиком из логического раздела диска</summary>

1. `sudo mkswap /dev/sdXN` создание раздела под SWAP
2. После создания его необходимо указать в /etc/fstab по UUID
  - `lsblk -f` - этой командой смотрим UUID у раздела SWAP
  -  `sudo nano /etc/fstab` - в fstab добавляем строчку
```
UUID=a9fa39fe-93a8-44eb-9520-e21a308993e7 /path/to/mount      none    swap    sw      0       0
```
3. `sudo swapon -a` применяет изменения для SWAP

</details>

<details>
  <summary>Создание SWAP из выделенного места на логическом диске</summary>

1. `sudo dd if=/dev/zero of=/swapfile bs=1G count=10` - создает файл swapfile в корне, размером 10Gb (10 блоков по 1 Gb)
    - `sudo fallocate -l 1G /swapfile` - альтернативный вариант создания swap файла swapfile размером 1Gb в корне диска
2. `sudo chmod 600 /swapfile` - выставить права для свап файла
3. `sudo mkswap /swapfile` - инициализация swap файла
4. `sudo swapon /swapfile` - включение свап файла
5. В /etc/fstab заполняем строку
```
/swapfile  none  swap  sw  0  0
```

</details>

</details>

`sudo mkfs.ext4 /dev/sd<X><N>` - команда для форматирования раздела `<N>` на диске `<X>` в файловую систему `ext4`

`sudo mount /dev/sdXN /` - команда для монтирования раздела `<N>` диска `<X>` в данном случае раздел монтируется в корень `</>`

<details>
  <summary>Увеличение размера раздела из свободного места на диске</summary>

1. `echo 1 > /sys/block/<disk>/device/rescan`  пересканирования SCSI-устройства `/dev/<disk>` (Такая команда может быть полезной, если, например, вы только что изменили размер диска виртуальной машины или добавили новое физическое SCSI-устройство, и хотите, чтобы система увидела
   изменения без необходимости перезагружать сервер)

```
sudo sh -c 'echo 1 > /sys/block/<disk>/device/rescan'
```

3. `sudo growpart /dev/<disk> <disk_part>` Увеличивает раздел `<disk_parted>` на свободное место диска `/dev/<disk>`
4. `sudo resize2fs /dev/<disk_part>` Изменяет размер файловой системы раздела на доступное неразмеченное место

</details>

`parted` - утилита командной строки для работы с разделами диска

<details>
  <summary>PARTED COMMAND</summary>

- mklabel / mkl - создать новую таблицу разделов на диске.
- mkpart / mkp - создать новый раздел на диске.
- rm - удалить указанный раздел.
- resizepart / rsp - изменить размер указанного раздела.
- print / p - вывести информацию о разделах на диске.
- set - установить значение для флага раздела.
- help / h - вывести справку по командам parted.
- quit / q - выйти из утилиты parted.

</details>
