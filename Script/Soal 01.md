### Soal 1

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
