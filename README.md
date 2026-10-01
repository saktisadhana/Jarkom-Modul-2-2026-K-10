## Identitas Praktikan

| Nama                     | NRP        | Kelas |
| ------------------------ | ---------- | ----- |
| Putu Putra Sakti Sadhana | 5027251101 | B     |
| Danish Faeyza Rusmawan   | 5027251028 | B     |
## Reporting
### Soal 1
#### Brief Overview

Konfigurasi `The Mesh` dan penentuaan IP address masing masing Node.

| Network / switch           | Entity  | Interface | IP address  | Mask | Gateway     |
| -------------------------- | ------- | --------- | ----------- | ---- | ----------- |
| 192.216.1.0/24 · Switch1-3 | prab    | eth0      | 192.216.1.2 | /24  | 192.216.1.1 |
|                            | tedd    | eth0      | 192.216.1.3 | /24  | 192.216.1.1 |
|                            | obladi  | eth0      | 192.216.1.4 | /24  | 192.216.1.1 |
|                            | desmond | eth0      | 192.216.1.5 | /24  | 192.216.1.1 |
|                            | oblada  | eth0      | 192.216.1.6 | /24  | 192.216.1.1 |
|                            | molly   | eth0      | 192.216.1.7 | /24  | 192.216.1.1 |
| 192.216.2.0/24 · Switch4   | abbey   | eth0      | 192.216.2.2 | /24  | 192.216.2.1 |
| 192.216.3.0/24 · Switch5   | penny   | eth0      | 192.216.3.2 | /24  | 192.216.3.1 |
| 192.216.4.0/24 · Switch6   | alpha   | eth0      | 192.216.4.2 | /24  | 192.216.4.1 |
|                            | beta    | eth0      | 192.216.4.3 | /24  | 192.216.4.1 |
|                            | gamma   | eth0      | 192.216.4.4 | /24  | 192.216.4.1 |
| 192.216.5.0/24 · Switch7   | delta   | eth0      | 192.216.5.2 | /24  | 192.216.5.1 |
|                            | epsilon | eth0      | 192.216.5.3 | /24  | 192.216.5.1 |

#### Step by Step

##### Setup Topologi

![](Topology.png)

Nodes `prab`, `tedd`, `abbey`, `penny`, `obladi`, `desmond`, `oblada`, `molly` adalah docker container `debinet` reskinned.

##### Network Configuration

###### rootkit

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
    address 192.216.1.1
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 192.216.2.1
    netmask 255.255.255.0

auto eth3
iface eth3 inet static
    address 192.216.3.1
    netmask 255.255.255.0

auto eth4
iface eth4 inet static
    address 192.216.4.1
    netmask 255.255.255.0

auto eth5
iface eth5 inet static
    address 192.216.5.1
    netmask 255.255.255.0
    
up ./setup_file.sh
```

###### prab

```
auto eth0
iface eth0 inet static
    address 192.216.1.2
    netmask 255.255.255.0
    gateway 192.216.1.1
    
up ./setup_file.sh
```

###### tedd

```
auto eth0
iface eth0 inet static
    address 192.216.1.3
    netmask 255.255.255.0
    gateway 192.216.1.1
    
up ./setup_file.sh
```

###### obladi

```
auto eth0
iface eth0 inet static
    address 192.216.1.4
    netmask 255.255.255.0
    gateway 192.216.1.1
    
up ./setup_file.sh
```

###### desmond

```
auto eth0
iface eth0 inet static
    address 192.216.1.5
    netmask 255.255.255.0
    gateway 192.216.1.1
    
up ./setup_file.sh
```

###### oblada

```
auto eth0
iface eth0 inet static
    address 192.216.1.6
    netmask 255.255.255.0
    gateway 192.216.1.1
    
up ./setup_file.sh
```

###### molly

```
auto eth0
iface eth0 inet static
    address 192.216.1.7
    netmask 255.255.255.0
    gateway 192.216.1.1
    
up ./setup_file.sh
```

###### abbey

```
auto eth0
iface eth0 inet static
    address 192.216.2.2
    netmask 255.255.255.0
    gateway 192.216.2.1
    
up ./setup_file.sh
```

###### penny

```
auto eth0
iface eth0 inet static
    address 192.216.3.2
    netmask 255.255.255.0
    gateway 192.216.3.1
    
up ./setup_file.sh
```

###### alpha

```
auto eth0
iface eth0 inet static
    address 192.216.4.2
    netmask 255.255.255.0
    gateway 192.216.4.1
    
up ./setup_file.sh
```

###### beta

```
auto eth0
iface eth0 inet static
    address 192.216.4.3
    netmask 255.255.255.0
    gateway 192.216.4.1
    
up ./setup_file.sh
```

###### gamma

```
auto eth0
iface eth0 inet static
    address 192.216.4.4
    netmask 255.255.255.0
    gateway 192.216.4.1
    
up ./setup_file.sh
```

###### delta

```
auto eth0
iface eth0 inet static
    address 192.216.5.2
    netmask 255.255.255.0
    gateway 192.216.5.1
    
up ./setup_file.sh
```

###### epsilon

```
auto eth0
iface eth0 inet static
    address 192.216.5.3
    netmask 255.255.255.0
    gateway 192.216.5.1
    
up ./setup_file.sh
```

Fungsi dari `up ./setup_file.sh`  adalah untuk menjalankan script konfigurasi setiap GNS3 dinyalakan.

Isinya akan selalu berbeda sesuai dengan progress kelompok kami.

### Soal 2
#### Brief Overview

Menghubungkan 13 entitas `The Mesh` (`prab`, `tedd`, `obladi`, `desmond`, `oblada`, `molly`, `abbey`, `penny`, `alpha`, `beta`, `gamma`, `delta`, `epsilon`) dengan NAT.
#### Step by Step
##### `rootkit` : Menghidupkan koneksi NAT dengan Router

```bash
ip link set eth0 up
udhcpc -i eth0
```

Dalam step ini kami menjalankan dua command, `ip` dan `udhcpc`.

	ip link set eth0 up

Berfungsi untuk mengaktifkan interface `eth0` pada router `rootkit` yang terhubung ke NAT. Sedangkan:

	udhcpc -i eth0

Berfungsi untuk meminta alamat IP secara otomatis dari NAT menggunakan DHCP client. Flag `-i eth0` menentukan bahwa permintaan DHCP dikirim lewat interface `eth0`.

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

![700](rootkit%20-%20ip%20forwarding.png)

###### Menghidupkan IP Forwarding

```bash
sysctl -w net.ipv4.ip_forward=1
```

`sysctl` diambil dari manual tools nya sendiri, adalah :

	"sysctl is used to modify kernel parameters at runtime."

salah satu parameter tersebut adalah `net.ipv4.ip_forward`. flag `-w` berfungsi untuk  menulis ulang nilai `net.ipv4.ip_forward` dari `0` menjadi `1` yang dimana menandakan bahwa IP Forwarding hidup. 

###### Memblokir forwarding selain yang akan dikonfigurasi

```bash
iptables -P FORWARD DROP
```

`iptables` adalah tools untuk mengatur aturan firewall di kernel Linux. Flag `-P` (policy) menentukan aksi default sebuah chain, yaitu aksi yang diambil kalau paket tidak cocok dengan aturan mana pun.

Chain `FORWARD` berisi paket yang hanya numpang lewat `rootkit`, contohnya paket dari `alpha` ke internet, atau dari `prab` ke `abbey`. Dengan policy `DROP`, semua paket tersebut dibuang kecuali ada aturan yang mengizinkannya. Cara ini biasa disebut *deny by default*: yang boleh lewat hanya traffic yang kami izinkan secara eksplisit di langkah-langkah berikutnya.

###### Mengizinkan balasan dari koneksi yang sudah diizinkan

```bash
iptables -C FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT 2>/dev/null ||
    iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

Policy `DROP` di atas juga ikut membuang paket balasan. Misalnya `alpha` melakukan ping ke `1.1.1.1`. Request-nya keluar lewat `eth4`, tapi balasannya masuk lewat `eth0`. Tanpa aturan ini, balasan tersebut dibuang dan ping tidak pernah dapat jawaban.

- `-m conntrack` memakai modul *connection tracking*, yang mencatat setiap koneksi yang lewat `rootkit`.
- `--ctstate ESTABLISHED,RELATED` mencocokkan paket yang merupakan bagian dari koneksi yang sudah berjalan (`ESTABLISHED`), atau paket yang berkaitan dengan koneksi tersebut (`RELATED`), contohnya pesan error ICMP.
- `-j ACCEPT` artinya paket yang cocok diizinkan lewat.

Aturan ini berlaku untuk balasan dari arah mana pun, jadi cukup ditulis sekali untuk semua LAN.

Command ditulis dengan pola `-C ... || -A ...`:

- `-C` (check) mengecek apakah aturan sudah ada di chain. Kalau sudah ada, command berhenti di sini.
- Kalau belum ada, `-C` gagal, lalu `||` menjalankan command di belakangnya, yaitu `-A` (append) yang menambahkan aturan ke akhir chain.
- `2>/dev/null` membuang pesan error dari `-C` saat aturan belum ada, supaya output tidak penuh.

Dengan pola ini, script aman dijalankan berulang kali (misalnya setiap node dinyalakan ulang) tanpa membuat aturan ganda.

###### Mengizinkan semua LAN untuk memulai koneksi

```bash
for lan in eth1 eth2 eth3 eth4 eth5; do
    iptables -C FORWARD -i "$lan" -j ACCEPT 2>/dev/null ||
        iptables -A FORWARD -i "$lan" -j ACCEPT
done
```

Loop `for` menjalankan aturan yang sama untuk kelima interface LAN (`eth1` sampai `eth5`). Flag `-i "$lan"` mencocokkan paket yang **masuk** dari interface tersebut. Jadi semua paket yang berasal dari LAN boleh diteruskan, baik ke internet (lewat `eth0`) maupun ke LAN lain.

Tidak ada aturan untuk `-i eth0`. Artinya koneksi baru dari arah NAT menuju LAN tetap diblokir oleh policy `DROP`. Yang boleh masuk dari `eth0` hanya paket balasan, sesuai aturan sebelumnya.

Pola `-C ... || -A ...` di sini sama seperti di atas.

###### Mengaktifkan NAT (MASQUERADE) untuk semua subnet LAN

```bash
iptables -t nat -C POSTROUTING -s 192.216.0.0/16 -o eth0 -j MASQUERADE 2>/dev/null ||
    iptables -t nat -A POSTROUTING -s 192.216.0.0/16 -o eth0 -j MASQUERADE
```

- `-t nat` memakai tabel `nat`. Tanpa flag ini, `iptables` memakai tabel `filter` (tempat chain `FORWARD` tadi).
- `POSTROUTING` adalah chain yang diproses tepat sebelum paket keluar dari `rootkit`.
- `-s 192.216.0.0/16` mencocokkan paket dari semua subnet `192.216.1.0/24` sampai `192.216.5.0/24`, jadi cukup satu aturan untuk kelima LAN.
- `-o eth0` membatasi aturan hanya untuk paket yang keluar lewat `eth0` (ke arah NAT).
- `-j MASQUERADE` mengganti IP sumber paket dengan IP `eth0` milik `rootkit`.

Aturan ini dibutuhkan karena NAT node GNS3 tidak tahu jalan ke jaringan `192.216.x.x`. Yang ia kenal hanya IP `rootkit` yang ia berikan lewat DHCP. Kalau paket dari `alpha` keluar dengan IP sumber `192.216.4.2`, balasannya tidak akan pernah kembali. Dengan MASQUERADE, semua paket terlihat seolah-olah dikirim oleh `rootkit`. Saat balasan datang, `rootkit` mengembalikan alamat tujuannya ke host asli berdasarkan catatan conntrack.

Kami memakai `MASQUERADE`, bukan `SNAT`, karena IP `eth0` didapat dari DHCP dan bisa berubah. `MASQUERADE` selalu memakai IP `eth0` yang sedang aktif, sedangkan `SNAT` butuh IP yang ditulis manual.

Traffic antar LAN (misalnya `prab` ke `abbey`) tidak terkena NAT, karena paketnya keluar lewat `eth2`, bukan `eth0`.

##### `rootkit` : Cek Konfigurasi

```bash
ping -c 3 1.1.1.1
```


### Soal 3
#### Brief Overview

Mengkonfigurasi resolver untuk setiap entitas / nodes non router (`prab`, `tedd`, `obladi`, `desmond`, `oblada`, `molly`, `abbey`, `penny`, `alpha`, `beta`, `gamma`, `delta`, `epsilon`) untuk terhubung ke IP DNS Server NAT (`192.168.122.1`) 

#### Step by Step

#####  `13 Hosts` : Setup Resolver

```bash
grep -qxF 'nameserver 192.168.122.1' /etc/resolv.conf || printf 'nameserver 192.168.122.1\n' >> /etc/resolv.conf
```

`192.168.122.1` adalah alamat NAT node GNS3. Untuk `rootkit`, alamat ini berperan sebagai gateway (di sisi `eth0`) sekaligus DNS. Untuk 13 entitas lain, gateway-nya tetap `rootkit`, sedangkan `192.168.122.1` hanya dipakai sebagai DNS server yang meneruskan query ke internet. `rootkit` tidak perlu disetting manual karena `udhcpc` sudah mengisi `/etc/resolv.conf` miliknya.

- `grep -qxF` mencari baris yang **persis sama** (`-x`) dengan teks biasa, bukan regex (`-F`), tanpa menampilkan output (`-q`).
- Kalau baris sudah ada, `grep` berhasil dan command selesai. Kalau belum ada, `||` menjalankan `printf ... >> /etc/resolv.conf` untuk menambahkannya di akhir file.

Sama seperti pola `-C ... || -A ...` di `iptables`, command ini aman dijalankan berulang kali tanpa membuat baris `nameserver` ganda.

##### `alpha` : Connectivity Test

```bash
ping -c 3 192.216.4.1
ping -c 3 192.216.1.2
ping -c 3 192.216.2.2
ping -c 3 192.216.3.2
ping -c 3 192.216.5.2
ping -c 3 1.1.1.1
```

Ping ke `192.216.4.1` menguji koneksi ke gateway di LAN sendiri. Ping ke `prab`, `abbey`, `penny`, dan `delta` menguji routing antar LAN lewat `rootkit`. Ping ke `1.1.1.1` menguji forwarding dan NAT ke internet. Ping ke `1.1.1.1`  untuk menguji DNS.

![](alpha%20-%20connectivity%20test.png)
### Soal 4
#### Brief Overview

Membuat DNS server untuk domain `k10.com`, dengan `prab` sebagai DNS master dan `tedd` sebagai DNS slave yang menyalin zona dari `prab`.

#### Step by Step

##### `prab` : Setup Domain & `dnsutil`

```bash
DOMAIN=k10.com
apt-get update
apt-get install -y bind9 bind9-utils dnsutils
```

![](prab%20-%20install%20bind9.png)

Dalam step ini kami menentukan nama domain lalu menginstal DNS server.

	DOMAIN=k10.com

Berfungsi untuk menyimpan nama domain ke variabel `DOMAIN`, supaya di command berikutnya cukup menulis `$DOMAIN`. Variabel ini hanya berlaku di terminal yang sedang dibuka. Sedangkan:

	apt-get install -y bind9 bind9-utils dnsutils

Berfungsi untuk menginstal tiga paket. `bind9` adalah DNS server-nya (programnya bernama `named`), `bind9-utils` berisi `named-checkconf` dan `named-checkzone` untuk mengecek konfigurasi, dan `dnsutils` berisi `dig` untuk pengujian. Flag `-y` membuat instalasi langsung berjalan tanpa meminta konfirmasi.

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

Dalam step ini kami menulis file `/etc/bind/named.conf.options`, yaitu file pengaturan umum `named`. Tanda kutip pada `'EOF'` membuat isi file ditulis apa adanya.

	listen-on { any; };
	listen-on-v6 { none; };

Berfungsi agar `named` menerima query di semua interface IPv4, dan tidak memakai IPv6 karena tidak dipakai di jaringan ini.

	allow-query { any; };

Berfungsi untuk mengizinkan siapa saja bertanya tentang zona `k10.com`.

	recursion yes;
	allow-recursion { 127.0.0.1; 192.216.0.0/16; };

Berfungsi agar `named` mau mencarikan nama di luar `k10.com`, tetapi hanya untuk `prab` sendiri dan jaringan `192.216.0.0/16`. Dengan begitu server ini tidak bisa dipakai sembarang orang sebagai *open resolver*.

	forwarders { 192.168.122.1; };
	forward only;

Berfungsi untuk meneruskan semua pertanyaan di luar `k10.com` ke NAT (`192.168.122.1`). `forward only` membuat `named` tidak mencoba mencari sendiri ke root server.

	dnssec-validation no;

Berfungsi untuk mematikan validasi DNSSEC. Forwarder NAT GNS3 tidak selalu meneruskan data DNSSEC dengan lengkap, sehingga kalau validasi dinyalakan, query ke domain luar bisa dijawab `SERVFAIL`.

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

Dalam step ini kami mendaftarkan zona `k10.com` di `/etc/bind/named.conf.local`. Heredoc `<<EOF` ditulis tanpa tanda kutip supaya `$DOMAIN` diganti menjadi `k10.com`.

	type primary;
	file "/etc/bind/db.$DOMAIN";

Berfungsi untuk menjadikan `prab` sebagai sumber data asli zona `k10.com`, dengan isi zona dibaca dari file `/etc/bind/db.k10.com`. Sedangkan:

	notify yes;
	also-notify { 192.216.1.3; };
	allow-transfer { 192.216.1.3; };

Berfungsi agar `prab` memberi tahu `tedd` setiap kali zona berubah, dan hanya `tedd` yang boleh menyalin seluruh isi zona.

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

Dalam step ini kami menulis isi zona `k10.com`. `@` adalah singkatan untuk nama zona itu sendiri, yaitu `k10.com`.

	\$TTL 3600

Berfungsi untuk menentukan bahwa jawaban dari zona ini boleh disimpan di cache selama 3600 detik. Tanda `\` dipakai supaya shell tidak menganggap `$TTL` sebagai variabel.

	@ IN SOA prab.$DOMAIN. hostmaster.$DOMAIN. (
	    2026092901 3600 900 604800 3600
	)

Berfungsi sebagai identitas zona. `prab.k10.com.` adalah server utamanya, dan `hostmaster.k10.com.` adalah email pengelola (`hostmaster@k10.com`). Lima angka di dalam kurung secara berurutan adalah:

| Angka | Nama | Arti |
| --- | --- | --- |
| `2026092901` | serial | versi zona, dengan format tanggal + nomor urut (`YYYYMMDDnn`) |
| `3600` | refresh | seberapa sering `tedd` mengecek apakah ada versi baru |
| `900` | retry | jeda sebelum `tedd` mencoba lagi kalau `prab` tidak bisa dihubungi |
| `604800` | expire | kalau `prab` tidak bisa dihubungi selama 7 hari, `tedd` berhenti menjawab untuk zona ini |
| `3600` | negative TTL | berapa lama jawaban "nama tidak ditemukan" boleh disimpan di cache |

	@    IN NS prab.$DOMAIN.
	@    IN NS tedd.$DOMAIN.

Berfungsi untuk menyatakan bahwa `prab` dan `tedd` adalah name server untuk `k10.com`. Sedangkan:

	prab IN A  192.216.1.2
	tedd IN A  192.216.1.3
	@    IN A  192.216.3.2

Berfungsi untuk menerjemahkan nama `prab` dan `tedd` ke IP-nya, dan membuat `k10.com` mengarah ke `penny` (`192.216.3.2`).

##### `prab` : cek file lalu jalankan server

```bash
named-checkconf
named-checkzone "$DOMAIN" "/etc/bind/db.$DOMAIN"
service named restart
service named status
```

Dalam step ini kami mengecek konfigurasi sebelum menjalankan `named`.

	named-checkconf
	named-checkzone "$DOMAIN" "/etc/bind/db.$DOMAIN"

Berfungsi untuk mengecek penulisan file konfigurasi (`named-checkconf`) dan isi file zona (`named-checkzone`). Kalau keduanya tidak menampilkan error, baru kami jalankan:

	service named restart
	service named status

Berfungsi untuk menjalankan ulang `named` supaya membaca konfigurasi yang baru, lalu menampilkan apakah `named` sedang berjalan.

##### `tedd` : tentukan domain yang sama dan instal `BIND9`

```bash
DOMAIN=k10.com
apt-get update
apt-get install -y bind9 bind9-utils dnsutils
```

Sama seperti di `prab`, kami menyimpan nama domain di variabel `DOMAIN` lalu menginstal `bind9`, `bind9-utils`, dan `dnsutils`.

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

Dalam step ini kami menulis `named.conf.options` di `tedd`. Isinya sama dengan `prab`, hanya tanpa `dnssec-validation no;`. `tedd` juga melayani query dari klien, jadi butuh forwarder yang sama.

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

Dalam step ini kami mendaftarkan zona `k10.com` di `tedd` sebagai salinan dari `prab`.

	type secondary;
	primaries { 192.216.1.2; };

Berfungsi untuk menjadikan `tedd` sebagai DNS slave yang menyalin zona dari `prab` (`192.216.1.2`). Sedangkan:

	file "/var/cache/bind/db.$DOMAIN";

Berfungsi untuk menentukan lokasi file salinan. Kami pakai `/var/cache/bind/` karena `named` berjalan sebagai user `bind`, dan user ini hanya boleh menulis di folder tersebut.

Di step ini kami memakai `>>`, yang menambahkan teks ke akhir file. Kalau dijalankan dua kali, zona `k10.com` akan terdaftar dua kali dan `named-checkconf` akan error. Untuk menjalankan ulang, lebih aman memakai `>`.

##### `tedd` : cek konfigurasi dan jalankan server

```bash
named-checkconf
service named restart
service named status
```

![](tedd%20-%20jalankan%20named.png)

Dalam step ini kami mengecek konfigurasi lalu menjalankan `named` di `tedd`, sama seperti di `prab`. Di output muncul pesan:

	rndc: connect failed: 127.0.0.1#953: connection refused

Pesan ini muncul karena `named` belum pernah jalan. Saat `apt-get install`, container memblokir service untuk start otomatis (`policy-rc.d denied execution of start`). Jadi saat `restart` mencoba menghentikan `named` lewat `rndc` (port 953), tidak ada yang menjawab. Setelah itu `named` tetap berhasil start, terlihat dari `bind is running`.

##### `prab` : uji apakah kedua DNS authoritative

```bash
dig @192.216.1.2 "$DOMAIN" A +norecurse
dig @192.216.1.3 "$DOMAIN" A +norecurse
dig @192.216.1.3 "prab.$DOMAIN" A +norecurse
dig @192.216.1.3 "tedd.$DOMAIN" A +norecurse
```

![620](prab%20-%20uji%20authoritative%20domain.png)
![](prab%20-%20uji%20authoritative%20prab%20dan%20tedd.png)

Dalam step ini kami menguji kedua DNS server memakai `dig`. `dig` diambil dari manual tools nya sendiri, adalah :

	"dig is a flexible tool for interrogating DNS name servers."

Contohnya:

	dig @192.216.1.3 "$DOMAIN" A +norecurse

Berfungsi untuk menanyakan A record `k10.com` ke `tedd`. `@192.216.1.3` menentukan server yang ditanya, dan flag `+norecurse` meminta server menjawab hanya dari data yang ia pegang sendiri.

Jawaban `prab` dan `tedd` sama-sama berstatus `NOERROR` dan memiliki flag `aa` (*authoritative answer*) di baris `flags:`. Artinya `tedd` sudah berhasil menyalin zona dari `prab`, dan kedua server sama-sama menjawab sebagai pemegang zona `k10.com` yang sah.

#### Kendala 
##### Script "`prab` : daftarkan zona sebagai primary" salah

Saya tidak sengaja menjalankan script milik `tedd` (DNS slave) di `prab`, sehingga `named.conf.local` di `prab` berisi:

```bash
cat >> /etc/bind/named.conf.local <<EOF
zone "$DOMAIN" {
    type secondary;
    primaries { 192.216.1.2; };
    file "/var/cache/bind/db.$DOMAIN";
};
EOF
```

Akibatnya `prab` menganggap dirinya secondary dan mencoba menyalin zona `k10.com` dari `192.216.1.2`, yaitu dari dirinya sendiri. File `/etc/bind/db.k10.com` tidak pernah dibaca, jadi `prab` menjawab `SERVFAIL`. `tedd` ikut menjawab `SERVFAIL` karena tidak punya sumber yang bisa disalin. Ini terlihat saat `named` dijalankan di foreground dengan `named -g -u bind`:

	zone k10.com/IN: refresh: unexpected rcode (SERVFAIL) from primary 192.216.1.2#53

##### Perbaikan

Mengganti `type secondary` menjadi `type primary` saja tidak cukup, karena baris `primaries` dan lokasi `file` juga milik konfigurasi secondary. Isi `named.conf.local` di `prab` kami tulis ulang dengan konfigurasi primary yang lengkap:

![](prab%20-%20perbaikan%20zona%20primary.png)

Lalu `named` di `prab` dijalankan ulang:

```bash
named-checkconf && service named restart && service named status
```

Setelah `prab` jalan, `tedd` diminta menyalin ulang zonanya:

```bash
rndc retransfer k10.com
```

### Soal 5
#### Brief Overview

Menambahkan A record untuk semua entitas The Mesh di zona `k10.com`, lalu memastikan perubahannya ikut tersalin ke `tedd`.

#### Step by Step

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

Dalam step ini kami menulis ulang file zona `k10.com` dengan 13 A record, satu untuk setiap entitas.

	2026092902 3600 900 604800 3600

Berfungsi untuk menaikkan serial dari `2026092901` menjadi `2026092902`. `tedd` hanya menyalin ulang zona kalau serial di `prab` lebih besar dari serial yang ia pegang, jadi tanpa kenaikan serial perubahan ini tidak akan sampai ke `tedd`. Sedangkan:

	obladi  IN A  192.216.1.4

Berfungsi untuk menerjemahkan `obladi.k10.com` menjadi `192.216.1.4`. Baris lainnya mengikuti pola yang sama untuk setiap entitas.

##### `prab` : cek file lalu set ulang

```bash
named-checkzone "$DOMAIN" "/etc/bind/db.$DOMAIN"
service named reload
```

![](prab%20-%20penamaan%20semua%20entitas.png)

Dalam step ini kami menjalankan dua command, `named-checkzone` dan `service`.

	named-checkzone "$DOMAIN" "/etc/bind/db.$DOMAIN"

Berfungsi untuk mengecek apakah isi file zona valid. Sedangkan:

	service named reload

Berfungsi untuk membuat `named` membaca ulang file zona tanpa harus dimatikan. Setelah reload, `prab` mengirim NOTIFY ke `tedd`, lalu `tedd` menyalin zona yang baru.

##### `prab` : cek zona baru apakah sudah ada di tedd

```bash
dig @192.216.1.2 alpha.k10.com A +norecurse
dig @192.216.1.3 alpha.k10.com A +norecurse
dig @192.216.1.3 epsilon.k10.com A +norecurse
```

![629](prab%20-%20cek%20alpha%20di%20prab%20dan%20tedd.png)
![](prab%20-%20cek%20epsilon%20di%20tedd.png)

Dalam step ini kami menanyakan `alpha.k10.com` ke `prab` dan `tedd`, serta `epsilon.k10.com` ke `tedd`, dengan cara yang sama seperti di Soal 4.

`tedd` menjawab kedua nama itu dengan flag `aa`, padahal record-nya baru ditambahkan di `prab`. Ini membuktikan zona yang baru sudah tersalin ke `tedd`.

### Soal 6
#### Brief Overview

Memastikan `prab` dan `tedd` memegang versi zona yang sama dengan membandingkan serial SOA.

#### Step by Step

##### `prab` & `tedd` : cek nilai serial SOA

```bash
dig @192.216.1.2 k10.com SOA +short | awk '{print "prab:", $3}'
dig @192.216.1.3 k10.com SOA +short | awk '{print "tedd:", $3}'
```

![](prab%20-%20cek%20serial%20soa.png)
![](tedd%20-%20cek%20serial%20soa.png)

Dalam step ini kami menanyakan record SOA ke kedua server, lalu hanya mengambil serial-nya.

	dig @192.216.1.2 k10.com SOA +short

Berfungsi untuk menanyakan record SOA `k10.com` ke `prab`. Flag `+short` membuat jawabannya tampil dalam satu baris:

	prab.k10.com. hostmaster.k10.com. 2026092902 3600 900 604800 3600

Sedangkan:

	awk '{print "prab:", $3}'

Berfungsi untuk mengambil kolom ketiga, yaitu serial, lalu memberi label nama server di depannya.

Kedua server menunjukkan serial `2026092902`, artinya `tedd` sudah memegang versi zona terbaru dari `prab`.

### Soal 7
#### Brief Overview

Menambahkan A record `vault` (ke `obladi` dan `desmond`) dan `core` (ke `oblada` dan `molly`), serta CNAME `www` ke `penny` dan `static` ke `abbey`, lalu memastikan jawaban `prab` dan `tedd` konsisten.

#### Step by Step

##### `prab` : A record vault & core, CNAME www & static

```bash
command -v named > /dev/null || { apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get install -y -qq bind9 dnsutils > /dev/null 2>&1; }
Z=/etc/bind/db.k10.com
OLD=$(grep -m1 -oE 'SOA[^(]*\( *[0-9]+' "$Z" 2> /dev/null | grep -oE '[0-9]+$'); TODAY=$(date +%Y%m%d)01
SERIAL=$(( ${OLD:-0} >= TODAY ? ${OLD:-0} + 1 : TODAY ))
EXTRA=$(grep -E '^(alpha|beta|gamma|delta|epsilon) +IN +TXT|^outbound +IN +CNAME' "$Z" 2> /dev/null)
cat > "$Z" << ZONE
\$TTL 3600
@ IN SOA prab.k10.com. hostmaster.k10.com. ( $SERIAL 3600 900 604800 3600 )
@        IN NS    prab.k10.com.
@        IN NS    tedd.k10.com.
@        IN A     192.216.3.2
prab     IN A     192.216.1.2
tedd     IN A     192.216.1.3
obladi   IN A     192.216.1.4
desmond  IN A     192.216.1.5
oblada   IN A     192.216.1.6
molly    IN A     192.216.1.7
abbey    IN A     192.216.2.2
penny    IN A     192.216.3.2
alpha    IN A     192.216.4.2
beta     IN A     192.216.4.3
gamma    IN A     192.216.4.4
delta    IN A     192.216.5.2
epsilon  IN A     192.216.5.3
; soal 7: vault = obladi + desmond, core = oblada + molly
vault    IN A     192.216.1.4
vault    IN A     192.216.1.5
core     IN A     192.216.1.6
core     IN A     192.216.1.7
www      IN CNAME penny.k10.com.
static   IN CNAME abbey.k10.com.
ZONE
[ -n "$EXTRA" ] && echo "$EXTRA" >> "$Z"
grep -q 'zone "k10.com"' /etc/bind/named.conf.local || cat >> /etc/bind/named.conf.local << 'CONF'
zone "k10.com" {
    type master;
    file "/etc/bind/db.k10.com";
    notify yes;
    also-notify { 192.216.1.3; };
    allow-transfer { 192.216.1.3; };
};
CONF
named-checkzone k10.com "$Z" > /dev/null && named-checkconf && \
{ { rndc reconfig && rndc reload; } > /dev/null 2>&1 || service named restart > /dev/null 2>&1 || service bind9 restart > /dev/null 2>&1; } && echo "prab OK, serial $SERIAL"
```

![](prab%20-%20record%20vault%20core%20www%20static.png)

Dalam step ini kami menulis ulang zona `k10.com` dengan record baru, lalu memuat ulang `named`.

	command -v named > /dev/null || { ... install -y -qq bind9 dnsutils ... }

Berfungsi untuk menginstal BIND hanya kalau `named` belum ada. `command -v` mengecek apakah program tersebut sudah terpasang.

	OLD=$(grep -m1 -oE 'SOA[^(]*\( *[0-9]+' "$Z" 2> /dev/null | grep -oE '[0-9]+$'); TODAY=$(date +%Y%m%d)01
	SERIAL=$(( ${OLD:-0} >= TODAY ? ${OLD:-0} + 1 : TODAY ))

Berfungsi untuk menghitung serial baru secara otomatis. `OLD` adalah serial lama, dan `TODAY` adalah tanggal hari ini ditambah `01`. Kalau serial lama sudah sama atau lebih besar dari `TODAY`, serial baru = serial lama + 1. Kalau belum, serial baru = `TODAY`. Dengan begitu serial selalu naik dan `tedd` selalu ikut menyalin.

	EXTRA=$(grep -E '^(alpha|beta|gamma|delta|epsilon) +IN +TXT|^outbound +IN +CNAME' "$Z" 2> /dev/null)

Berfungsi untuk menyimpan TXT record dan CNAME `outbound` kalau sudah ada di zona, supaya bisa ditulis kembali setelah zona ditulis ulang. Di soal ini keduanya belum ada, jadi `EXTRA` kosong.

	vault    IN A     192.216.1.4
	vault    IN A     192.216.1.5
	core     IN A     192.216.1.6
	core     IN A     192.216.1.7

Berfungsi untuk membuat `vault.k10.com` mengarah ke `obladi` dan `desmond`, serta `core.k10.com` ke `oblada` dan `molly`. Karena satu nama punya dua IP, DNS mengembalikan keduanya dan klien memakai salah satunya. Ini cara sederhana membagi pengunjung ke dua server (*round robin DNS*). Sedangkan:

	www      IN CNAME penny.k10.com.
	static   IN CNAME abbey.k10.com.

Berfungsi untuk menjadikan `www.k10.com` nama lain dari `penny.k10.com`, dan `static.k10.com` nama lain dari `abbey.k10.com`.

	grep -q 'zone "k10.com"' /etc/bind/named.conf.local || cat >> /etc/bind/named.conf.local << 'CONF'

Berfungsi untuk mendaftarkan zona hanya kalau belum ada. Di `prab` zona sudah terdaftar sejak Soal 4, jadi bagian ini dilewati. `type master` di dalamnya adalah nama lama untuk `type primary`.

	{ rndc reconfig && rndc reload; }

Berfungsi untuk membuat `named` membaca ulang konfigurasi dan zona tanpa dimatikan. Command ini hanya jalan kalau `named-checkzone` dan `named-checkconf` tidak menemukan error. Kalau `rndc` gagal, `named` di-restart lewat `service`.

##### `tedd` : slave untuk k10.com

```bash
command -v named > /dev/null || { apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get install -y -qq bind9 dnsutils > /dev/null 2>&1; }
grep -q 'zone "k10.com"' /etc/bind/named.conf.local || cat >> /etc/bind/named.conf.local << 'CONF'
zone "k10.com" {
    type slave;
    masters { 192.216.1.2; };
    file "/var/cache/bind/db.k10.com";
};
CONF
named-checkconf && { { rndc reconfig && rndc retransfer k10.com; } > /dev/null 2>&1 || service named restart > /dev/null 2>&1 || service bind9 restart > /dev/null 2>&1; }
sleep 2; echo "tedd OK, serial $(dig +short @127.0.0.1 k10.com SOA | awk '{print $3}')"
```

![](tedd%20-%20slave%20untuk%20k10.png)

Dalam step ini kami memastikan `tedd` memegang salinan zona yang baru. Zona `k10.com` sudah terdaftar di `tedd` sejak Soal 4, jadi yang benar-benar dijalankan hanya:

	rndc retransfer k10.com

Berfungsi untuk memaksa `tedd` menyalin ulang zona dari `prab` saat itu juga. Baris terakhir script menampilkan serial yang sekarang dipegang `tedd`, yang harus sama dengan serial di `prab`.

##### `alpha` & `beta` : verifikasi dari dua klien

```bash
echo "Klien: $(cat /etc/hostname 2> /dev/null)"
printf '%-18s %-28s %-28s %s\n' NAMA "prab (192.216.1.2)" "tedd (192.216.1.3)" STATUS
for N in vault core www static penny abbey obladi desmond oblada molly; do
  A=$(dig +short @192.216.1.2 $N.k10.com | sort | paste -sd ' ')
  B=$(dig +short @192.216.1.3 $N.k10.com | sort | paste -sd ' ')
  [ -n "$A" ] && [ "$A" = "$B" ] && S=KONSISTEN || S=BEDA
  printf '%-18s %-28s %-28s %s\n' "$N.k10.com" "$A" "$B" "$S"
done
```

Dalam step ini kami menjalankan blok yang sama di `alpha` lalu di `beta`.

	A=$(dig +short @192.216.1.2 $N.k10.com | sort | paste -sd ' ')
	B=$(dig +short @192.216.1.3 $N.k10.com | sort | paste -sd ' ')

Berfungsi untuk mengambil jawaban `prab` (`A`) dan `tedd` (`B`) untuk setiap nama. `sort` mengurutkan jawaban karena urutan IP `vault` dan `core` bisa berganti, dan `paste -sd ' '` menggabungkannya menjadi satu baris. Sedangkan:

	[ -n "$A" ] && [ "$A" = "$B" ] && S=KONSISTEN || S=BEDA

Berfungsi untuk memberi status `KONSISTEN` kalau jawaban kedua server tidak kosong dan sama persis.

Untuk `www` dan `static`, hasil `dig` berisi nama tujuan CNAME (`penny.k10.com.` atau `abbey.k10.com.`) dan IP akhirnya.

### Soal 8
#### Brief Overview

Membuat reverse DNS (PTR) untuk IP `abbey`, `penny`, serta backend vault dan core di `prab`, lalu menyalinnya ke `tedd`, dengan jawaban yang *authoritative*.

#### Step by Step

##### `prab` : reverse zone + PTR untuk abbey, penny, vault, core

```bash
SERIAL=$(date +%Y%m%d)01
for SEG in 1 2 3; do
  F=/etc/bind/db.mesh.rev.$SEG
  OLD=$(grep -m1 -oE 'SOA[^(]*\( *[0-9]+' "$F" 2> /dev/null | grep -oE '[0-9]+$')
  S=$(( ${OLD:-0} >= SERIAL ? ${OLD:-0} + 1 : SERIAL ))
  printf '$TTL 3600\n@ IN SOA prab.k10.com. hostmaster.k10.com. ( %s 3600 900 604800 3600 )\n@ IN NS prab.k10.com.\n@ IN NS tedd.k10.com.\n' "$S" > "$F"
done
cat >> /etc/bind/db.mesh.rev.1 << 'EOF2'
; area vault (obladi, desmond) dan area core (oblada, molly)
4 IN PTR vault.k10.com.
5 IN PTR vault.k10.com.
6 IN PTR core.k10.com.
7 IN PTR core.k10.com.
EOF2
echo '2 IN PTR abbey.k10.com.' >> /etc/bind/db.mesh.rev.2
echo '2 IN PTR penny.k10.com.' >> /etc/bind/db.mesh.rev.3
for SEG in 1 2 3; do
  grep -q "zone \"$SEG.216.192.in-addr.arpa\"" /etc/bind/named.conf.local || cat >> /etc/bind/named.conf.local << CONF
zone "$SEG.216.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.mesh.rev.$SEG";
    notify yes;
    also-notify { 192.216.1.3; };
    allow-transfer { 192.216.1.3; };
};
CONF
  named-checkzone $SEG.216.192.in-addr.arpa /etc/bind/db.mesh.rev.$SEG > /dev/null || echo "zona reverse $SEG SALAH"
done
named-checkconf && { { rndc reconfig && rndc reload; } > /dev/null 2>&1 || service named restart > /dev/null 2>&1 || service bind9 restart > /dev/null 2>&1; } && echo "prab OK: reverse zone 1, 2, 3"
```

Dalam step ini kami membuat tiga reverse zone di `prab`. Nama reverse zone dibuat dari bagian network IP yang urutannya dibalik, lalu ditambah `.in-addr.arpa`. Contohnya, network `192.216.1.0/24` menjadi `1.216.192.in-addr.arpa`.

| Reverse zone | Network | Isi |
| --- | --- | --- |
| `1.216.192.in-addr.arpa` | `192.216.1.0/24` | `obladi`, `desmond`, `oblada`, `molly` |
| `2.216.192.in-addr.arpa` | `192.216.2.0/24` | `abbey` |
| `3.216.192.in-addr.arpa` | `192.216.3.0/24` | `penny` |

	printf '$TTL 3600\n@ IN SOA prab.k10.com. hostmaster.k10.com. ( %s 3600 900 604800 3600 )\n@ IN NS prab.k10.com.\n@ IN NS tedd.k10.com.\n' "$S" > "$F"

Berfungsi untuk menulis bagian awal setiap file zona (TTL, SOA, dan NS). Serial `$S` dihitung dengan cara yang sama seperti di Soal 7.

	4 IN PTR vault.k10.com.

Berfungsi untuk menerjemahkan `192.216.1.4` menjadi `vault.k10.com`. Di dalam reverse zone, nama record cukup oktet terakhir IP. `obladi` dan `desmond` sama-sama diarahkan ke `vault`, sedangkan `oblada` dan `molly` ke `core`.

	zone "$SEG.216.192.in-addr.arpa" {
	    type master;

Berfungsi untuk mendaftarkan ketiga reverse zone sebagai master di `named.conf.local`, dengan pengaturan `notify` dan `allow-transfer` yang sama seperti zona `k10.com`. Setelah itu setiap file zona dicek dengan `named-checkzone`, dan `rndc reconfig` membuat `named` memuat zona-zona baru tersebut.

##### `tedd` : tarik reverse zone sebagai slave

```bash
for SEG in 1 2 3; do
  grep -q "zone \"$SEG.216.192.in-addr.arpa\"" /etc/bind/named.conf.local || cat >> /etc/bind/named.conf.local << CONF
zone "$SEG.216.192.in-addr.arpa" {
    type slave;
    masters { 192.216.1.2; };
    file "/var/cache/bind/db.mesh.rev.$SEG";
};
CONF
done
named-checkconf && { { rndc reconfig && for SEG in 1 2 3; do rndc retransfer $SEG.216.192.in-addr.arpa; done; } > /dev/null 2>&1 || service named restart > /dev/null 2>&1 || service bind9 restart > /dev/null 2>&1; }
sleep 2; echo "tedd OK: reverse zone ditarik dari prab"
```

Dalam step ini kami mendaftarkan ketiga reverse zone di `tedd` sebagai slave dengan sumber `prab`, sama seperti zona `k10.com` di Soal 4.

	rndc retransfer $SEG.216.192.in-addr.arpa

Berfungsi untuk memaksa `tedd` langsung menyalin setiap reverse zone dari `prab`.

##### `alpha` : bukti PTR dijawab authoritative oleh prab dan tedd

```bash
for S in 192.216.1.2 192.216.1.3; do
  echo "=== server $S ==="
  for IP in 192.216.2.2 192.216.3.2 192.216.1.4 192.216.1.5 192.216.1.6 192.216.1.7; do
    OUT=$(dig +norecurse @$S -x $IP)
    PTR=$(echo "$OUT" | awk '$4 == "PTR" {print $5}')
    echo "$OUT" | grep -qE '^;; flags:.* aa[ ;]' && AA="authoritative (aa)" || AA="TIDAK authoritative"
    printf '%-12s -> %-18s %s\n' "$IP" "$PTR" "$AA"
  done
done
```

Dalam step ini kami menanyakan PTR keenam IP ke `prab` dan `tedd`.

	OUT=$(dig +norecurse @$S -x $IP)

Berfungsi untuk melakukan reverse lookup. Flag `-x` membuat `dig` otomatis mengubah IP menjadi nama reverse-nya, misalnya `192.216.1.4` menjadi `4.1.216.192.in-addr.arpa`. Sedangkan:

	PTR=$(echo "$OUT" | awk '$4 == "PTR" {print $5}')
	echo "$OUT" | grep -qE '^;; flags:.* aa[ ;]' && AA="authoritative (aa)" || AA="TIDAK authoritative"

Berfungsi untuk mengambil nama tujuan dari baris jawaban PTR, lalu mengecek apakah ada flag `aa` di baris `flags:`.

Kedua server harus menjawab keenam IP dengan nama yang sama dan berstatus `authoritative (aa)`.

### Soal 9
#### Brief Overview

Menjalankan web statis `vault.k10.com` di `obladi` dan `desmond` memakai Apache, dengan directory listing (autoindex) di folder `/arsip/`.

#### Step by Step

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

Dalam step ini kami menginstal Apache, membuat isi web, lalu menulis konfigurasi situs `vault.k10.com`.

	mkdir -p /var/www/mesh-vault/arsip

Berfungsi untuk membuat folder web `/var/www/mesh-vault` beserta folder `arsip/` di dalamnya. Folder ini lalu diisi `index.html` dan dua file contoh, `catatan.txt` dan `data.csv`.

	LOGFMT=$(grep -oE 'mesh-vault-access\.log +[A-Za-z0-9]+' ... | awk '{print $2}'); LOGFMT=${LOGFMT:-combined}

Berfungsi untuk mempertahankan format access log kalau config situs ini sudah pernah diubah. Kalau belum, dipakai format bawaan `combined`.

	ServerName vault.k10.com
	ServerAlias obladi.k10.com

Berfungsi untuk menentukan nama utama situs, yaitu `vault.k10.com`, dan menambahkan `obladi.k10.com` sebagai nama lain.

	<Directory /var/www/mesh-vault>
	    Options -Indexes +FollowSymLinks

Berfungsi untuk mematikan directory listing di folder utama. Sedangkan:

	<Directory /var/www/mesh-vault/arsip>
	    Options +Indexes
	    DirectoryIndex disabled
	    IndexOptions FancyIndexing HTMLTable

Berfungsi untuk menyalakan directory listing khusus di folder `/arsip`. `DirectoryIndex disabled` memastikan yang tampil selalu daftar file walaupun ada `index.html`, dan `IndexOptions FancyIndexing HTMLTable` menampilkan daftar file dalam bentuk tabel lengkap dengan ukuran dan tanggal.

	ErrorLog \${APACHE_LOG_DIR}/mesh-vault-error.log

Tanda `\` berfungsi supaya `${APACHE_LOG_DIR}` tidak diganti oleh shell, karena variabel ini nanti dibaca oleh Apache sendiri.

	echo "ServerName obladi.k10.com" > /etc/apache2/conf-available/k10-servername.conf

Berfungsi untuk mengisi `ServerName` global, supaya Apache tidak menampilkan peringatan `Could not reliably determine the server's fully qualified domain name`.

	chmod -R a+rX /var/www/mesh-vault

Berfungsi agar semua file bisa dibaca oleh user `www-data` yang menjalankan Apache.

	a2enmod autoindex > /dev/null; a2enconf k10-servername > /dev/null; a2ensite mesh-vault > /dev/null; a2dissite 000-default > /dev/null 2>&1

Berfungsi untuk mengaktifkan modul `autoindex`, config `ServerName`, dan situs `mesh-vault`, lalu mematikan situs bawaan Apache (`000-default`).

	apache2ctl configtest && apache2ctl -k graceful && echo "Apache OK"

Berfungsi untuk mengecek config, lalu memuat ulang Apache tanpa memutus koneksi yang sedang berjalan. Kalau Apache belum jalan, `graceful` akan menyalakannya.

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

Sama seperti di `obladi`. Bedanya hanya nama node di `index.html`, `catatan.txt`, `ServerAlias`, dan `ServerName` global, supaya saat pengujian terlihat node mana yang menjawab.

##### `alpha` : arahkan resolver ke `prab` dan `tedd`

```bash
printf 'nameserver 192.216.1.2\nnameserver 192.216.1.3\nnameserver 192.168.122.1\n' > /etc/resolv.conf
```

Dalam step ini kami mengganti isi `/etc/resolv.conf` di `alpha`.

	printf 'nameserver 192.216.1.2\nnameserver 192.216.1.3\nnameserver 192.168.122.1\n' > /etc/resolv.conf

Berfungsi untuk menjadikan `prab` dan `tedd` sebagai DNS pertama dan kedua, dengan `192.168.122.1` sebagai cadangan. Sejak Soal 3, `alpha` hanya memakai `192.168.122.1`, yang tidak tahu zona `k10.com` buatan kami. Tanpa step ini, nama seperti `vault.k10.com` tidak akan ditemukan, atau malah dijawab dengan data domain `k10.com` yang asli di internet.

##### `alpha` : bukti directory listing lewat hostname

```bash
echo "=== http://vault.k10.com/arsip/ ==="
curl -s http://vault.k10.com/arsip/ | grep -oE '<title>[^<]*</title>|href="[a-z][^"]*"'
for IP in 192.216.1.4 192.216.1.5; do
  curl -s -o /dev/null -w "vault.k10.com/arsip/ lewat $IP -> %{http_code}\n" --resolve vault.k10.com:80:$IP http://vault.k10.com/arsip/
done
```

Dalam step ini kami membuka `/arsip/` dari `alpha` memakai `curl`.

	curl -s http://vault.k10.com/arsip/ | grep -oE '<title>[^<]*</title>|href="[a-z][^"]*"'

Berfungsi untuk membuka `/arsip/` lewat nama `vault.k10.com`, lalu hanya mengambil judul halaman dan link file. Flag `-s` menyembunyikan progress bar `curl`. Judul `Index of /arsip` beserta link `catatan.txt` dan `data.csv` membuktikan directory listing aktif. Sedangkan:

	curl -s -o /dev/null -w "... %{http_code}\n" --resolve vault.k10.com:80:$IP http://vault.k10.com/arsip/

Berfungsi untuk mengirim request ke satu node tertentu. Flag `--resolve` membuat request tetap memakai hostname `vault.k10.com`, tetapi dikirim ke IP yang kami tentukan. Flag `-o /dev/null` membuang isi halaman, dan `-w` hanya menampilkan kode HTTP. `obladi` dan `desmond` sama-sama harus menjawab `200`.

### Soal 10
#### Brief Overview

Menjalankan web dinamis `core.k10.com` di `oblada` dan `molly` memakai nginx dan PHP-FPM, dengan halaman profil yang bisa dibuka lewat URL `/profil` tanpa akhiran `.php`.

#### Step by Step

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

Dalam step ini kami menginstal nginx dan PHP-FPM, membuat dua halaman PHP, lalu menulis konfigurasi situs `core.k10.com`. nginx tidak bisa menjalankan PHP sendiri, jadi file `.php` diteruskan ke PHP-FPM.

	V=$(ls /usr/sbin/php-fpm[0-9]* | sed 's|.*php-fpm||' | sort -V | tail -n 1)

Berfungsi untuk mendeteksi versi PHP-FPM dari nama programnya. Misalnya `/usr/sbin/php-fpm8.4` menghasilkan `V=8.4`. Versi ini dipakai untuk nama socket dan nama service.

	echo "oblada" > /var/www/mesh-core/node.txt

Berfungsi untuk menyimpan nama node. Isinya ditampilkan oleh `index.php` dan `profil.php` lewat `<?= ... ?>`, supaya terlihat node mana yang melayani request.

	listen 80 default_server;
	root /var/www/mesh-core;
	index index.php;

Berfungsi agar server block ini menjawab semua request di port 80, dengan folder web `/var/www/mesh-core` dan `index.php` sebagai halaman utama.

	location = /profil {
	    rewrite ^ /profil.php last;
	}

Berfungsi untuk membuat URL bersih `/profil`. `location = /profil` hanya cocok untuk URL `/profil`, lalu `rewrite` mengubahnya secara internal menjadi `/profil.php`. `last` membuat nginx mencari ulang `location` yang cocok, yaitu `location ~ \.php$`, sehingga request diteruskan ke PHP-FPM. Perubahan ini terjadi di dalam server, jadi URL di sisi klien tetap `/profil` dan tidak ada redirect.

	location ~ \.php\$ {
	    try_files \$uri =404;
	    include fastcgi_params;
	    fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
	    fastcgi_pass unix:/run/php/php$V-fpm.sock;
	}

Berfungsi untuk meneruskan semua file `.php` ke PHP-FPM lewat socket. `try_files \$uri =404` menolak file PHP yang tidak ada, dan `SCRIPT_FILENAME` memberi tahu PHP-FPM lokasi file yang harus dijalankan. Tanda `\` di depan variabel nginx seperti `\$uri` berfungsi supaya tidak ikut diganti oleh shell, karena heredoc ini sengaja ditulis tanpa tanda kutip agar `$V` terisi versi PHP.

	location = /node.txt { deny all; }

Berfungsi untuk menutup akses langsung ke `node.txt`.

	rm -f /etc/nginx/sites-enabled/default
	ln -sf /etc/nginx/sites-available/mesh-core /etc/nginx/sites-enabled/mesh-core

Berfungsi untuk mematikan situs bawaan nginx dan mengaktifkan situs `mesh-core`.

	service "php$V-fpm" restart
	[ -S "/run/php/php$V-fpm.sock" ] || { "php-fpm$V" -D; sleep 1; }

Berfungsi untuk menjalankan PHP-FPM. Kalau socket-nya tetap tidak muncul, program `php-fpm` dijalankan langsung di background dengan flag `-D`.

	nginx -t && { nginx -s reload 2> /dev/null || nginx; }

Berfungsi untuk mengecek config nginx, lalu memuat ulang nginx, atau menyalakannya kalau belum jalan.

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

Sama seperti di `oblada`. Bedanya hanya isi `node.txt` dan `server_name`.

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

Dalam step ini kami membuka beranda dan `/profil` dari `alpha`.

	curl -s -w "HTTP %{http_code}\n" "http://core.k10.com$P" | grep -oE '<h1>[^<]*</h1>|Node: [a-z]+|HTTP [0-9]+'

Berfungsi untuk membuka `/` dan `/profil` lewat nama `core.k10.com`, lalu mengambil judul halaman (`<h1>`), nama node, dan kode HTTP. Halaman `/profil` yang menampilkan `<h1>Profil</h1>` dengan kode `200` membuktikan URL bersih sudah berjalan. Nilai `Node:` bisa berganti antara `oblada` dan `molly`, karena `core.k10.com` mengarah ke kedua node.

Loop kedua memakai `--resolve` seperti di Soal 9, supaya `/profil` di `oblada` dan `molly` sama-sama teruji lewat hostname.

### Soal 11
#### Brief Overview

#### Step by Step

### Soal 12
#### Brief Overview

#### Step by Step

### Soal 13
#### Brief Overview

#### Step by Step

### Soal 14
#### Brief Overview

#### Step by Step

### Soal 15
#### Brief Overview

#### Step by Step

### Soal 16
#### Brief Overview

#### Step by Step

### Soal 17
#### Brief Overview

#### Step by Step

### Soal 18
#### Brief Overview

#### Step by Step

### Soal 19
#### Brief Overview

#### Step by Step

### Soal 20
#### Brief Overview

#### Step by Step

