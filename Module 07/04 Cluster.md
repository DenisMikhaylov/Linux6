Интеграция контейнеров с менеджером кластера

```
crm configure
```
```
primitive pr_lxc_server1 systemd:lxc@server1
group gr_fs_lxc pr_fs_r0 pr_lxc_server1
commit
quit
```
Проверяем работу кластера
```
crm status
```
