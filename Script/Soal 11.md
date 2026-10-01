### Soal 11

##### `obladi` & `desmond` : Konfigurasi Backend Vault

```bash
Header always set X-Backend "NAMA_BACKEND"
Header always set X-Got-Host "expr=%{req:Host}"
Header always set X-Got-Real-IP "expr=%{req:X-Real-IP}"
```

```bash
LogFormat "%h realip=%{X-Real-IP}i host=%{Host}i \"%r\" %>s" k10proof
```

```bash
apache2ctl configtest
```

##### `oblada` & `molly` : Konfigurasi Backend Core

```bash
add_header X-Backend "NAMA_BACKEND" always;
add_header X-Got-Host $host always;
add_header X-Got-Real-IP $http_x_real_ip always;
```

```bash
log_format k10proof
'$remote_addr realip=$http_x_real_ip host=$host "$request" $status';

access_log /var/log/nginx/k10-proof.log k10proof;
```

```bash
nginx -t
```

##### `penny` : Konfigurasi Reverse Proxy

```bash
<Proxy "balancer://vault">
    BalancerMember "http://192.216.1.4"
    BalancerMember "http://192.216.1.5"
    ProxySet lbmethod=byrequests
</Proxy>

ProxyPreserveHost On

RequestHeader set X-Real-IP "expr=%{REMOTE_ADDR}"

ProxyPass "/" "balancer://vault/"
ProxyPassReverse "/" "balancer://vault/"
```

##### `abbey` : Konfigurasi Reverse Proxy

```bash
upstream core_backend {
    zone core_backend 64k;
    server 192.216.1.6;
    server 192.216.1.7;
}

server {
    listen 80 default_server;
    server_name static.k10.com abbey.k10.com;

    location / {
        proxy_pass http://core_backend;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        add_header X-Gateway "abbey" always;
    }
}
```

##### `alpha` : Pengujian Penny

```bash
for i in 1 2 3 4 5 6; do
    curl -s -o /dev/null -D - http://www.k10.com/ |
    tr -d '\r' |
    grep -iE '^x-(gateway|backend|got-real-ip|got-host)' |
    paste -sd ' ';
done
```

##### `alpha` : Pengujian Abbey

```bash
for i in 1 2 3 4 5 6; do
    curl -s -o /dev/null -D - http://static.k10.com/ |
    tr -d '\r' |
    grep -iE '^x-(gateway|backend|got-real-ip|got-host)' |
    paste -sd ' ';
done
```
