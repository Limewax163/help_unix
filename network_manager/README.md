# NETWORK MANAGER

При настройке VPN соединения через network-manager-fortisslvpn-gnome, при включении параметра "Использовать это подключение только для ресурсов в этой сети" (Используется для того чтобы например для других доменов VPN не использовался) необходимо дополнительно вручную прописать ДНС адреса и параметры:

`nmcli connection modify 'UUID' ipv4.never-default true` - чтобы сеть никогда не использовалась по умолчанию
`nmcli connection modify 'UUID' ipv4.dns-search "vpn_domain.ru"` пропишем явно в какой домен будем ходить через VPN
`nmcli connection modify 'UUID' ipv4.dns "ip DNS server"` (опционально) прописать статично ДНСы

`nmcli connection show` Посмотреть все существующие подключения
