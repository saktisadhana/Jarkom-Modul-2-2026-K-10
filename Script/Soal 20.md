### Soal 20

##### Konfigurasi `autostart.sh`

```bash
#!/bin/bash

IP=192.216.3.2
GW=192.216.3.1

ip link set eth0 up
ip addr replace $IP/24 dev eth0
ip route replace default via $GW dev eth0

printf 'nameserver 192.216.1.2\nnameserver 192.216.1.3\nnameserver 192.168.122.1\n' > /etc/resolv.conf

apache2ctl configtest > /dev/null 2>&1 &&
apache2ctl -k graceful > /dev/null 2>&1 ||
apache2ctl -k start > /dev/null 2>&1
```

##### Autostart

```bash
chmod +x /root/autostart.sh
```

##### `alpha` : Verifikasi setelah restart

```bash
for U in www static; do
    for i in 1 2; do
        curl -s -o /dev/null -D - \
        http://$U.k10.com/ |
        tr -d '\r' |
        grep -iE '^x-(gateway|backend|got-real-ip)' |
        paste -sd ' ';
    done
done
```
