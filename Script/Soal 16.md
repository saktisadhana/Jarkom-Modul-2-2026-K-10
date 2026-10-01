### Soal 16

##### `alpha` : Pengujian

```bash
for URL in \
http://www.k10.com/ \
http://static.k10.com/; do

echo "================ $URL ================"

ab -l -n 250 -c 10 "$URL" 2> /dev/null |
grep -E '^(Server Software|Concurrency Level|Time taken|Complete requests|Failed requests|Non-2xx|Requests per second|Time per request|Transfer rate)'

done
```
