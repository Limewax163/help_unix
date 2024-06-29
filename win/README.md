### Windows

- `netsh wlan show network` - посмотреть список доступных сетей
- `netsh wlan show profile` - посмотреть список сохраненных сетей
- `netsh wlan show networks` - посмотреть доступные сети (сканирование)
- `netsh connect name=<name>` - подключить уже сохраненную(с паролем) сеть. Но если надо подключить новую то добавить `ssid=<ssid> password=<password> соль=<перец> по вкусу`

> [!NOTE]
> Процесс настройки подключения по ssh ключу в Шиндовс к учетным записям администраторов(пользователей состоящих в группах администраторов) отличается от доступа по ssh ключу обычных смертных пользователей

<details>
  <summary>Доступ к аднмиским аккаунтам по ssh (идиотизм)</summary>

1. Настраиваем сам ssh сервер. В конфиге `C:\ProgramData\ssh\sshd_config` интересуют всего две позиции `<PasswordAuthentication>` `<PubkeyAuthentication>`
   - прописываем соответсвующим образом (зависит от того что нужно)
```
PasswordAuthentication <no or yes>
PubkeyAuthentication yes
```
2. Создать файл специально для администраторов `<administrators_authorized_keys>` куда добавляем свою публичную(открытую) часть ключа сервака откуда собираемся подключаться к Шиндовс
3. Все файлы по классике должны иметь определенные права чтобы все работало как нужно. Но здесь два пути
   - можно поменять права на папки и сделать все "как надо"
```
icacls.exe "C:\ProgramData\ssh\administrators_authorized_keys" /inheritance:r /grant "Администраторы:F" /grant "СИСТЕМА:F"
```
  - либо в конфиге установить значение `<StrictModes>` в `<no>`
```
StrictModes no
```
4. Рестартануть сервис ssh (OpenSSH SSH Server)

</details>

<details>
  <summary>Почистить WinSxS</summary>

Лучше выполнять в том порядке в котором указано, за исключением первых двух команд (StartComponentCleanup через планировщик либо через dism)

- StartComponentCleanup в планировщике задач для очистки и сжатия компонентов
```
schtasks.exe /Run /TN "\Microsoft\Windows\Servicing\StartComponentCleanup"
```
- Использование параметра Dism.exe `/StartComponentCleanup` (предыдущие версии обновленных компонентов будут немедленно удалены, в планировщике задан слип 1ч)
```
Dism.exe /online /Cleanup-Image /StartComponentCleanup
```
- Удаляет все замененные версии каждого компонента в хранилище компонентов (много чистит)
```
Dism.exe /online /Cleanup-Image /StartComponentCleanup /ResetBase
```

</details>
