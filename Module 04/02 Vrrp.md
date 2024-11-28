На Node1
```
nano /etc/network/interfaces
```
```
auto eth0
iface eth0 inet manual
        up ip link set eth0 up

auto eth1
iface eth0 inet static
        address 192.168.10.1
        netmask 255.255.255.0
        gateway 192.168.10.1
```

```
node1# nano /etc/keepalived/keepalived.conf
```
```
global_defs {
router_id uMASTER
enable_script_security
script_user root
}
vrrp_script myhealth {
    script "/bin/nc -z -w 2 127.0.0.1 53"
    interval 10
    user nobody
}
vrrp_instance VI_1 {

    state MASTER
    interface br0
    priority 90
    advert_int 1
    nopreempt

        authentication {
        auth_type PASS
        auth_pass secret
        }

    virtual_router_id 1
    virtual_ipaddress {
        192.168.10.254
    }
    virtual_routes {
        0.0.0.0/0 via 192.168.123.2 dev eth0
    }
    track_script {
        myhealth
    }
}
```
На Node2
```
nano /etc/network/interfaces
```
```
auto eth0
iface eth0 inet manual
        up ip link set eth0 up

auto eth1
iface eth0 inet static
        address 192.168.10.2
        netmask 255.255.255.0
        gateway 192.168.10.1
```

```
node2# nano /etc/keepalived/keepalived.conf
```
```
global_defs {
router_id uBACKUP
enable_script_security
script_user root
}
vrrp_script myhealth {
    script "/bin/nc -z -w 2 127.0.0.1 53"
    interval 10
    user nobody
}
vrrp_instance VI_1 {

    state BACKUP
    interface br0
    priority 90
    advert_int 1
    nopreempt

        authentication {
        auth_type PASS
        auth_pass secret
        }

    virtual_router_id 1
    virtual_ipaddress {
        192.168.10.254
    }
    virtual_routes {
        0.0.0.0/0 via 192.168.123.2 dev eth0
    }
    track_script {
        myhealth
    }
}
```

Мониторинг

```
node1# systemctl stop keepalived
```
```
node1# ip -brief address show
```

```
node2# ip -brief address show
```
