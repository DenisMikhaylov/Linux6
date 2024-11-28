Установка Linux контейнерам

```
node1# apt install lxc
```
```
node2# apt install lxc
```

Настройка сети
```
node1# cat /etc/default/lxc-net
node2# cat /etc/default/lxc-net

```
```
node1# rm /etc/default/lxc-net
node2# rm /etc/default/lxc-net
```
```
node1:~# rmdir /var/lib/lxc/
node2:~# rmdir /var/lib/lxc/
```

Создаем линк для DRDB
```
node1# ln -s /disk2/var/lib/lxc/ /var/lib/lxc
node2# ln -s /disk2/var/lib/lxc/ /var/lib/lxc

```
Создаем каталог на первом узле
```
node1.corp.ru:~# mkdir -p /disk2/var/lib/lxc/
```

Создание контейнера

```
node1# lxc-create -t debian -n server-template
```

Настройка шаблона
```
Установка ПО в дочерней системе/шаблоне
```
меняем строку приветствия что бы понимать где мы находимся
```
PS1='server-template:\w# '
```
```
apt update
```
Производим установку и удаление ПО
```
apt purge netplan.io isc-dhcp-client

apt install nano ssh iputils-ping sudo
```

Меняем имя
```
nano /etc/hostname
```
```
server-template.corp.ru
```
Настройка hosts
```
nano /etc/hosts
```
```
192.168.10.30 server-template.corp.ru server-template
```

Проверяем resolve
```
nano /etc/resolved.conf
```
Создаем пользователя
```
useradd -m -s /bin/bash -G sudo student
```
```
passwd student
```
Выходим из шаблона
```
exit
```
Настраиваем lxc для запуска гостевой системы/шаблона в контейнере
```
nano /var/lib/lxc/server-template/config
```
Заменить
```
lxc.net.0.type = veth
lxc.net.0.link = br0
lxc.net.0.flags = up
lxc.net.0.ipv4.address = 192.168.10.30/24
lxc.net.0.ipv4.gateway = 192.168.10.254
```
