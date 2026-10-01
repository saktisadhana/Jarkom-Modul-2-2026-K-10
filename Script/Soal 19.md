### Soal 19

##### Konfigurasi

```bash
outbound IN CNAME http.badssl.com.
```

##### Verifikasi DNS

```bash
dig +noall +answer outbound.k10.com
```

##### Pengujian

```bash
curl -s http://outbound.k10.com/ > /tmp/outbound.html
curl -s http://http.badssl.com/ > /tmp/badssl.html
```

```bash
curl -s -H "Host: http.badssl.com" \
http://outbound.k10.com/ > /tmp/outbound.html
```
