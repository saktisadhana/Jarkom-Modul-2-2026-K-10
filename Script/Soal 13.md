### Soal 13

##### `alpha` : Pengujian

```bash
for URL in \
http://192.216.3.2/ \
http://penny.k10.com/ \
http://penny.k10.com/arsip/ \
http://192.216.2.2/ \
http://abbey.k10.com/ \
http://abbey.k10.com/profil \
http://www.k10.com/ \
http://static.k10.com/; do

printf '%-30s -> %s\n' "$URL" \
"$(curl -s -o /dev/null -w '%{http_code} %{redirect_url}' "$URL")"

done
```
