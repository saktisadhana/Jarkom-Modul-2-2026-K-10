### Soal 4

##### `prab` : Setup Domain & `dnsutil`

```bash
DOMAIN=k10.com
apt-get update
apt-get install -y bind9 bind9-utils dnsutils
```

##### `prab` : membuat script mengatur DNS

```bash
cat > /etc/bind/named.conf.options <<'EOF'
options {
    directory "/var/cache/bind";
    listen-on { any; };
    listen-on-v6 { none; };
    allow-query { any; };
    recursion yes;
    allow-recursion { 127.0.0.1; 192.216.0.0/16; };
    forwarders { 192.168.122.1; };
    forward only;
    dnssec-validation no;
};
EOF
```

##### `prab` : daftarkan zona sebagai primary

```bash
cat > /etc/bind/named.conf.local <<EOF
zone "$DOMAIN" {
    type primary;
    file "/etc/bind/db.$DOMAIN";
    notify yes;
    also-notify { 192.216.1.3; };
    allow-transfer { 192.216.1.3; };
};
EOF
```

##### `prab` : tulis isi zona dan A record

```bash
cat > "/etc/bind/db.$DOMAIN" <<EOF
\$TTL 3600
@ IN SOA prab.$DOMAIN. hostmaster.$DOMAIN. (
    2026092901 3600 900 604800 3600
)
@    IN NS prab.$DOMAIN.
@    IN NS tedd.$DOMAIN.
prab IN A  192.216.1.2
tedd IN A  192.216.1.3
@    IN A  192.216.3.2
EOF
```

##### `prab` : cek file lalu jalankan server

```bash
named-checkconf
named-checkzone "$DOMAIN" "/etc/bind/db.$DOMAIN"
service named restart
service named status
```

##### `tedd` : atur forwarder untuk pertanyaan di luar zona

```bash
cat > /etc/bind/named.conf.options <<'EOF'
options {
    directory "/var/cache/bind";
    listen-on { any; };
    listen-on-v6 { none; };
    allow-query { any; };
    recursion yes;
    allow-recursion { 127.0.0.1; 192.216.0.0/16; };
    forwarders { 192.168.122.1; };
    forward only;
};
EOF
```

##### `tedd` : BIND mengambil zona `k10.com`

```bash
cat >> /etc/bind/named.conf.local <<EOF
zone "$DOMAIN" {
    type secondary;
    primaries { 192.216.1.2; };
    file "/var/cache/bind/db.$DOMAIN";
};
EOF
```

##### `tedd` : cek konfigurasi dan jalankan server

```bash
named-checkconf
service named restart
service named status
```

##### `prab` : uji apakah kedua DNS authoritative

```bash
dig @192.216.1.2 "$DOMAIN" A +norecurse
dig @192.216.1.3 "$DOMAIN" A +norecurse
dig @192.216.1.3 "prab.$DOMAIN" A +norecurse
dig @192.216.1.3 "tedd.$DOMAIN" A +norecurse
```
