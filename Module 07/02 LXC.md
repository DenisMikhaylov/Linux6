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

