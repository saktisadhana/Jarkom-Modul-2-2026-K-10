### Soal 10

##### `oblada` : nginx + PHP-FPM, beranda + /profil

```bash
command -v nginx > /dev/null && ls /usr/sbin/php-fpm[0-9]* > /dev/null 2>&1 || { apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get install -y -qq nginx php-fpm > /dev/null 2>&1; }
V=$(ls /usr/sbin/php-fpm[0-9]* | sed 's|.*php-fpm||' | sort -V | tail -n 1)
mkdir -p /var/www/mesh-core
echo "oblada" > /var/www/mesh-core/node.txt
cat > /var/www/mesh-core/index.php << 'PHP'
<!doctype html><html lang="id"><meta charset="utf-8"><title>Core</title>
<h1>Beranda Core</h1>
<p>Node: <?= htmlspecialchars(trim(file_get_contents(__DIR__.'/node.txt'))) ?></p>
<p>Waktu server: <?= date('Y-m-d H:i:s') ?></p>
<a href="/profil">Buka profil</a></html>
PHP
cat > /var/www/mesh-core/profil.php << 'PHP'
<!doctype html><html lang="id"><meta charset="utf-8"><title>Profil</title>
<h1>Profil</h1><p>Nama aplikasi: The Mesh</p>
<p>Node: <?= htmlspecialchars(trim(file_get_contents(__DIR__.'/node.txt'))) ?></p>
<a href="/">Kembali ke beranda</a></html>
PHP
cat > /etc/nginx/sites-available/mesh-core << CONF
server {
    listen 80 default_server;
    server_name core.k10.com oblada.k10.com;
    root /var/www/mesh-core;
    index index.php;
    location / {
        try_files \$uri \$uri/ =404;
    }
    # soal 10: URL bersih /profil -> profil.php
    location = /profil {
        rewrite ^ /profil.php last;
    }
    location ~ \.php\$ {
        try_files \$uri =404;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
        fastcgi_pass unix:/run/php/php$V-fpm.sock;
    }
    location = /node.txt { deny all; }
}
CONF
chmod -R a+rX /var/www/mesh-core
rm -f /etc/nginx/sites-enabled/default
ln -sf /etc/nginx/sites-available/mesh-core /etc/nginx/sites-enabled/mesh-core
mkdir -p /run/php; service "php$V-fpm" restart > /dev/null 2>&1; sleep 1
[ -S "/run/php/php$V-fpm.sock" ] || { "php-fpm$V" -D; sleep 1; }
nginx -t && { nginx -s reload 2> /dev/null || nginx; } && sleep 1 && echo "nginx + PHP-FPM OK (php$V)"
```

##### `molly` : nginx + PHP-FPM, beranda + /profil

```bash
command -v nginx > /dev/null && ls /usr/sbin/php-fpm[0-9]* > /dev/null 2>&1 || { apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get install -y -qq nginx php-fpm > /dev/null 2>&1; }
V=$(ls /usr/sbin/php-fpm[0-9]* | sed 's|.*php-fpm||' | sort -V | tail -n 1)
mkdir -p /var/www/mesh-core
echo "molly" > /var/www/mesh-core/node.txt
cat > /var/www/mesh-core/index.php << 'PHP'
<!doctype html><html lang="id"><meta charset="utf-8"><title>Core</title>
<h1>Beranda Core</h1>
<p>Node: <?= htmlspecialchars(trim(file_get_contents(__DIR__.'/node.txt'))) ?></p>
<p>Waktu server: <?= date('Y-m-d H:i:s') ?></p>
<a href="/profil">Buka profil</a></html>
PHP
cat > /var/www/mesh-core/profil.php << 'PHP'
<!doctype html><html lang="id"><meta charset="utf-8"><title>Profil</title>
<h1>Profil</h1><p>Nama aplikasi: The Mesh</p>
<p>Node: <?= htmlspecialchars(trim(file_get_contents(__DIR__.'/node.txt'))) ?></p>
<a href="/">Kembali ke beranda</a></html>
PHP
cat > /etc/nginx/sites-available/mesh-core << CONF
server {
    listen 80 default_server;
    server_name core.k10.com molly.k10.com;
    root /var/www/mesh-core;
    index index.php;
    location / {
        try_files \$uri \$uri/ =404;
    }
    # soal 10: URL bersih /profil -> profil.php
    location = /profil {
        rewrite ^ /profil.php last;
    }
    location ~ \.php\$ {
        try_files \$uri =404;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
        fastcgi_pass unix:/run/php/php$V-fpm.sock;
    }
    location = /node.txt { deny all; }
}
CONF
chmod -R a+rX /var/www/mesh-core
rm -f /etc/nginx/sites-enabled/default
ln -sf /etc/nginx/sites-available/mesh-core /etc/nginx/sites-enabled/mesh-core
mkdir -p /run/php; service "php$V-fpm" restart > /dev/null 2>&1; sleep 1
[ -S "/run/php/php$V-fpm.sock" ] || { "php-fpm$V" -D; sleep 1; }
nginx -t && { nginx -s reload 2> /dev/null || nginx; } && sleep 1 && echo "nginx + PHP-FPM OK (php$V)"
```

##### `alpha` : bukti beranda dan /profil lewat hostname

```bash
for P in / /profil; do
  echo "=== http://core.k10.com$P ==="
  curl -s -w "HTTP %{http_code}\n" "http://core.k10.com$P" | grep -oE '<h1>[^<]*</h1>|Node: [a-z]+|HTTP [0-9]+'
done
for IP in 192.216.1.6 192.216.1.7; do
  curl -s -o /dev/null -w "core.k10.com/profil lewat $IP -> %{http_code}\n" --resolve core.k10.com:80:$IP http://core.k10.com/profil
done
```
