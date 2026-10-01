### Soal 8

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
