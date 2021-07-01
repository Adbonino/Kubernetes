```
[abonino@server1 ~]$ sudo mkdir /var/dbfiles
[abonino@server1 ~]$ sudo chmod -R 777 /var/dbfiles
[abonino@server1 ~]$ sudo semanage fcontext -a -t container_file_t '/var/dbfiles(/.*)?'
[abonino@server1 ~]$ sudo restorecon -Rv /var/dbfiles
Relabeled /var/dbfiles from unconfined_u:object_r:var_t:s0 to unconfined_u:object_r:container_file_t:s0
[abonino@server1 ~]$ ls -dZ /var/dbfiles
unconfined_u:object_r:container_file_t:s0 /var/dbfiles
[abonino@server1 ~]$ sudo podman run  --name mysql-basic  \
> -v /var/dbfiles:/var/lib/mysql/data \
> -e MYSQL_USER=abonino -e MYSQL_PASSWORD=jackfruitbus437 \
> -e MYSQL_DATABASE=itmes -e MYSQL_ROOT_PASSWORD=r00tpa55 \
-d docker.io/centos/mysql-57-centos7
adcef9da784ab445e731f6e90272305e466b92a0fe7c36052762baf3119289f2
[abonino@server1 ~]$ sudo podman ps --format="table {{.ID}} {{.Names}} {{.Status}}"
e3ea2d059309  mysql-basic  Up About a minute ago
[abonino@server1 ~]$ ls -l /var/dbfiles
total 41036
-rw-r-----. 1 27 27       56 Jul  1 19:56 auto.cnf
-rw-------. 1 27 27     1680 Jul  1 19:56 ca-key.pem
-rw-r--r--. 1 27 27     1112 Jul  1 19:56 ca.pem
-rw-r--r--. 1 27 27     1112 Jul  1 19:56 client-cert.pem
-rw-------. 1 27 27     1676 Jul  1 19:56 client-key.pem
-rw-r-----. 1 27 27        2 Jul  1 19:57 e3ea2d059309.pid
-rw-r-----. 1 27 27      372 Jul  1 19:57 ib_buffer_pool
-rw-r-----. 1 27 27 12582912 Jul  1 19:57 ibdata1
-rw-r-----. 1 27 27  8388608 Jul  1 19:57 ib_logfile0
-rw-r-----. 1 27 27  8388608 Jul  1 19:56 ib_logfile1
-rw-r-----. 1 27 27 12582912 Jul  1 19:57 ibtmp1
drwxr-x---. 2 27 27       20 Jul  1 19:56 itmes
drwxr-x---. 2 27 27     4096 Jul  1 19:56 mysql
-rw-r--r--. 1 27 27        6 Jul  1 19:56 mysql_upgrade_info
drwxr-x---. 2 27 27     8192 Jul  1 19:56 performance_schema
-rw-------. 1 27 27     1680 Jul  1 19:56 private_key.pem
-rw-r--r--. 1 27 27      452 Jul  1 19:56 public_key.pem
-rw-r--r--. 1 27 27     1112 Jul  1 19:56 server-cert.pem
-rw-------. 1 27 27     1680 Jul  1 19:56 server-key.pem
drwxr-x---. 2 27 27     8192 Jul  1 19:56 sys
```
