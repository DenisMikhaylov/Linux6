Создание системы из шаблона

Создаем переменные
```
SRC_CONTAINER=server-template
DST_CONTAINER=server1

SRC_IP=192.168.10.30
DST_IP=192.168.10.31
```
Создаем шаблон новой машины
```
time cp -rp /var/lib/lxc/$SRC_CONTAINER/ /var/lib/lxc/$DST_CONTAINER/

find /var/lib/lxc/$DST_CONTAINER/rootfs/etc/ -type f -exec sed -i'' -e "s/$SRC_CONTAINER/$DST_CONTAINER/" -e "s/$SRC_IP/$DST_IP/" {} \;

sed -i'' -e "s/$SRC_CONTAINER/$DST_CONTAINER/" -e "s/$SRC_IP/$DST_IP/" /var/lib/lxc/$DST_CONTAINER/config
```

Тестируем работу виртуальной системе на MASTER узле
```
lxc-info -n server1
```
Запустим машину
```
systemctl start lxc@server1
```
Проверяем что машина запустилась

```
lxc-info -n server1
```
Проверяем работы этой машины
```
putty ip address
```
Пользователь student

Останавливаем работу машины
```
systemctl stop lxc@server1
```
