### Soal 3

#####  `13 Hosts` : Setup Resolver

```bash
grep -qxF 'nameserver 192.168.122.1' /etc/resolv.conf || printf 'nameserver 192.168.122.1\n' >> /etc/resolv.conf
```

##### `alpha` : Connectivity Test

```bash
ping -c 3 192.216.4.1
ping -c 3 192.216.1.2
ping -c 3 192.216.2.2
ping -c 3 192.216.3.2
ping -c 3 192.216.5.2
ping -c 3 1.1.1.1
```
