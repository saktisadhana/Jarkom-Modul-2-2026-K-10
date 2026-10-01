### Soal 14

##### `obladi`, `desmond`, `oblada`, `molly` : Konfigurasi

```bash
RemoteIPHeader X-Real-IP
RemoteIPInternalProxy 192.216.3.2
```

```bash
set_real_ip_from 192.216.2.2;
real_ip_header X-Real-IP;
```

##### `alpha` : Kirim request lewat gateway

```bash
for i in 1 2 3 4; do
    curl -s -o /dev/null http://www.k10.com/
    curl -s -o /dev/null http://static.k10.com/
done

echo "8 request terkirim"
```

##### `obladi`/`desmond` & `oblada`/`molly` : Cek log

```bash
tail -n 4 /var/log/apache2/mesh-vault-access.log
```

```bash
tail -n 4 /var/log/nginx/mesh-core-access.log
```
