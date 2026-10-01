### Soal 6

##### `prab` & `tedd` : cek nilai serial SOA

```bash
dig @192.216.1.2 k10.com SOA +short | awk '{print "prab:", $3}'
dig @192.216.1.3 k10.com SOA +short | awk '{print "tedd:", $3}'
```
