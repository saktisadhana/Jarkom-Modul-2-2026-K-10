### Soal 17

##### `prab` : Konfigurasi TXT record

```bash
alpha IN TXT "alpha"
beta IN TXT "beta"
gamma IN TXT "gamma"
delta IN TXT "delta"
epsilon IN TXT "epsilon"
```

##### `alpha` : Pengujian

```bash
for H in alpha beta gamma delta epsilon; do
    printf '%-8s prab=%-10s tedd=%s\n' \
    "$H" \
    "$(dig +short @192.216.1.2 $H.k10.com TXT)" \
    "$(dig +short @192.216.1.3 $H.k10.com TXT)"
done
```
