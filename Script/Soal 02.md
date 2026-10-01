### Soal 2

##### `rootkit` : Menghidupkan koneksi NAT dengan Router

```bash
ip link set eth0 up
udhcpc -i eth0
```

##### `rootkit` : IP Forwarding

```bash
sysctl -w net.ipv4.ip_forward=1
iptables -P FORWARD DROP
iptables -C FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT 2>/dev/null ||
    iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
for lan in eth1 eth2 eth3 eth4 eth5; do
    iptables -C FORWARD -i "$lan" -j ACCEPT 2>/dev/null ||
        iptables -A FORWARD -i "$lan" -j ACCEPT
done
iptables -t nat -C POSTROUTING -s 192.216.0.0/16 -o eth0 -j MASQUERADE 2>/dev/null ||
    iptables -t nat -A POSTROUTING -s 192.216.0.0/16 -o eth0 -j MASQUERADE
```

##### `rootkit` : Cek Konfigurasi

```bash
ping -c 3 1.1.1.1
```
