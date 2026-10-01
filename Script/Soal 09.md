### Soal 9

##### `obladi` : Apache + autoindex /arsip/

```bash
command -v apache2 > /dev/null || { apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apache2 > /dev/null 2>&1; }
mkdir -p /var/www/mesh-vault/arsip
cat > /var/www/mesh-vault/index.html << 'HTML'
<!doctype html><html lang="id"><meta charset="utf-8"><title>Vault</title>
<h1>Vault: obladi</h1><p>Web statis Apache.</p><a href="/arsip/">Buka arsip</a></html>
HTML
echo "Arsip dokumen dari obladi" > /var/www/mesh-vault/arsip/catatan.txt
printf 'nama,status\ncontoh,tersedia\n' > /var/www/mesh-vault/arsip/data.csv
LOGFMT=$(grep -oE 'mesh-vault-access\.log +[A-Za-z0-9]+' /etc/apache2/sites-available/mesh-vault.conf 2> /dev/null | awk '{print $2}'); LOGFMT=${LOGFMT:-combined}
cat > /etc/apache2/sites-available/mesh-vault.conf << CONF
<VirtualHost *:80>
    ServerName vault.k10.com
    ServerAlias obladi.k10.com
    DocumentRoot /var/www/mesh-vault
    <Directory /var/www/mesh-vault>
        Options -Indexes +FollowSymLinks
        Require all granted
    </Directory>
    # soal 9: directory listing untuk /arsip/
    <Directory /var/www/mesh-vault/arsip>
        Options +Indexes
        DirectoryIndex disabled
        IndexOptions FancyIndexing HTMLTable
        Require all granted
    </Directory>
    ErrorLog \${APACHE_LOG_DIR}/mesh-vault-error.log
    CustomLog \${APACHE_LOG_DIR}/mesh-vault-access.log $LOGFMT
</VirtualHost>
CONF
echo "ServerName obladi.k10.com" > /etc/apache2/conf-available/k10-servername.conf
chmod -R a+rX /var/www/mesh-vault
a2enmod autoindex > /dev/null; a2enconf k10-servername > /dev/null; a2ensite mesh-vault > /dev/null; a2dissite 000-default > /dev/null 2>&1
apache2ctl configtest && apache2ctl -k graceful && echo "Apache OK"
```

##### `desmond` : Apache + autoindex /arsip/

```bash
command -v apache2 > /dev/null || { apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apache2 > /dev/null 2>&1; }
mkdir -p /var/www/mesh-vault/arsip
cat > /var/www/mesh-vault/index.html << 'HTML'
<!doctype html><html lang="id"><meta charset="utf-8"><title>Vault</title>
<h1>Vault: desmond</h1><p>Web statis Apache.</p><a href="/arsip/">Buka arsip</a></html>
HTML
echo "Arsip dokumen dari desmond" > /var/www/mesh-vault/arsip/catatan.txt
printf 'nama,status\ncontoh,tersedia\n' > /var/www/mesh-vault/arsip/data.csv
LOGFMT=$(grep -oE 'mesh-vault-access\.log +[A-Za-z0-9]+' /etc/apache2/sites-available/mesh-vault.conf 2> /dev/null | awk '{print $2}'); LOGFMT=${LOGFMT:-combined}
cat > /etc/apache2/sites-available/mesh-vault.conf << CONF
<VirtualHost *:80>
    ServerName vault.k10.com
    ServerAlias desmond.k10.com
    DocumentRoot /var/www/mesh-vault
    <Directory /var/www/mesh-vault>
        Options -Indexes +FollowSymLinks
        Require all granted
    </Directory>
    # soal 9: directory listing untuk /arsip/
    <Directory /var/www/mesh-vault/arsip>
        Options +Indexes
        DirectoryIndex disabled
        IndexOptions FancyIndexing HTMLTable
        Require all granted
    </Directory>
    ErrorLog \${APACHE_LOG_DIR}/mesh-vault-error.log
    CustomLog \${APACHE_LOG_DIR}/mesh-vault-access.log $LOGFMT
</VirtualHost>
CONF
echo "ServerName desmond.k10.com" > /etc/apache2/conf-available/k10-servername.conf
chmod -R a+rX /var/www/mesh-vault
a2enmod autoindex > /dev/null; a2enconf k10-servername > /dev/null; a2ensite mesh-vault > /dev/null; a2dissite 000-default > /dev/null 2>&1
apache2ctl configtest && apache2ctl -k graceful && echo "Apache OK"
```

##### `alpha` : arahkan resolver ke `prab` dan `tedd`

```bash
printf 'nameserver 192.216.1.2\nnameserver 192.216.1.3\nnameserver 192.168.122.1\n' > /etc/resolv.conf
```

##### `alpha` : bukti directory listing lewat hostname

```bash
echo "=== http://vault.k10.com/arsip/ ==="
curl -s http://vault.k10.com/arsip/ | grep -oE '<title>[^<]*</title>|href="[a-z][^"]*"'
for IP in 192.216.1.4 192.216.1.5; do
  curl -s -o /dev/null -w "vault.k10.com/arsip/ lewat $IP -> %{http_code}\n" --resolve vault.k10.com:80:$IP http://vault.k10.com/arsip/
done
```
