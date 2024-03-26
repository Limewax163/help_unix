>[!NOTE]
> За наполнение "приветствия" в консоли отвечают скрипты, которые находятся в папке:
> /etc/update-motd.d/

<details>
  <summary>Для добавления информационной статистики по docker контейнерам</summary>

```
#!/bin/sh

[ ! -x "/usr/bin/docker" ] || echo "Running docker containers:" && docker ps --format 'table {{.Names}}\t{{.Status}}\t{{.Command}}' --no-trunc && echo "\n"
```

</details>
