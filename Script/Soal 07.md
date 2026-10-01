### Soal 7

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
