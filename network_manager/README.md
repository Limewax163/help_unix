# NETWORK MANAGER

При настройке VPN соединения через network-manager-fortisslvpn-gnome, при включении параметра "Использовать это подключение только для ресурсов в этой сети" необходимо дополнительно вручную прописать ДНС адреса и параметры:

```
❯ nmcli connection modify '4e710c8d-75c6-417b-9c1c-459248152a6c' ipv4.never-default true
❯ nmcli connection modify '4e710c8d-75c6-417b-9c1c-459248152a6c' ipv4.dns-search "vpn_domain.ru"
❯ nmcli connection modify '4e710c8d-75c6-417b-9c1c-459248152a6c' ipv4.dns "ip DNS server"
```
