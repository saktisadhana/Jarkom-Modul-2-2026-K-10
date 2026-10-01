### Soal 5

##### `prab` : penamaan semua entitas

```bash
cat > "/etc/bind/db.$DOMAIN" <<EOF
\$TTL 3600
@ IN SOA prab.$DOMAIN. hostmaster.$DOMAIN. (
    2026092902 3600 900 604800 3600
)
@       IN NS prab.$DOMAIN.
@       IN NS tedd.$DOMAIN.

@       IN A  192.216.3.2
prab    IN A  192.216.1.2
tedd    IN A  192.216.1.3
obladi  IN A  192.216.1.4
desmond IN A  192.216.1.5
oblada  IN A  192.216.1.6
molly   IN A  192.216.1.7
abbey   IN A  192.216.2.2
penny   IN A  192.216.3.2
alpha   IN A  192.216.4.2
beta    IN A  192.216.4.3
gamma   IN A  192.216.4.4
delta   IN A  192.216.5.2
epsilon IN A  192.216.5.3
EOF
```

##### `prab` : cek file lalu set ulang

```bash
named-checkzone "$DOMAIN" "/etc/bind/db.$DOMAIN"
service named reload
```

##### `prab` : cek zona baru apakah sudah ada di tedd

```bash
dig @192.216.1.2 alpha.k10.com A +norecurse
dig @192.216.1.3 alpha.k10.com A +norecurse
dig @192.216.1.3 epsilon.k10.com A +norecurse
```
