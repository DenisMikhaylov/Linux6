
Установка keepalived

```
apt install keepalived
```
Node1
Настройка
Node 1
```
nodeN# nano /etc/keepalived/keepalived.conf
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
    priority 100
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
Node 2
```
nodeN# nano /etc/keepalived/keepalived.conf
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
```
nodeN# nano /usr/local/bin/vrrp.sh
```
```
#!/bin/sh

#echo $1 >> /tmp/vrrp.txt

case $1 in
    MASTER)
        ip route delete default
        ip route add default via 192.168.123.2
    ;;
    BACKUP)
        ip route delete default
        ip route add default via 192.168.10.254
    ;;
esac
```
```
nodeN# chmod +x /usr/local/bin/vrrp.sh
```

Запуск и мониторинг

```
# keepalived -t

# service keepalived restart

# watch "service keepalived status | cat"

# watch ipvsadm -L -n

# ipvsadm -L -n -c
```
