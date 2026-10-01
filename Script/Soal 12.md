### Soal 12

##### `alpha` : Pengujian `/admin`

```bash
curl -s -o /dev/null -w "tanpa kredensial -> %{http_code}\n" \
http://www.k10.com/admin/
```

```bash
curl -s -o /dev/null -w "password salah -> %{http_code}\n" \
-u 'prabs:salah' http://www.k10.com/admin/
```

```bash
curl -s -o /dev/null -w "prabs + password benar -> %{http_code}\n" \
-u 'prabs:pakar_pinter_jadi_gob***' \
http://www.k10.com/admin/
```
