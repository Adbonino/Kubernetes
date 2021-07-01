```
 [abonino@server1 ~]$ sudo podman run  --name mysql-basic \
 > -e MYSQL_USER=abonino \
 > -e MYSQL_PASSWORD=jackfruitbus437 \
 > -e MYSQL_DATABASE=itmes \
 > -e MYSQL_ROOT_PASSWORD=r00tpa55 \
 > -d docker.io/centos/mysql-57-centos7

[abonino@server1 ~]$ sudo podman ps
[sudo] password for abonino:
CONTAINER ID  IMAGE                              COMMAND     CREATED         STATUS             PORTS   NAMES
0eb754faef40  docker.io/centos/mysql-57-centos7  run-mysqld  14 minutes ago  Up 14 minutes ago          mysql-basic

[abonino@server1 ~]$ sudo podman exec -it mysql-basic /bin/bash
bash-4.2$ mysql -u root
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| itmes              |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> use itmes;
Database changed
mysql> show tables;
+-----------------+
| Tables_in_itmes |
+-----------------+
| Projects        |
+-----------------+
1 row in set (0.00 sec)

mysql> insert into Projects (id,name,code) values (1,'Developers','DO180');
mysql> select * from Projects;
+----+------------+-------+
| id | name       | code  |
+----+------------+-------+
|  1 | DevOps     | DO180 |
|  2 | Developers | DO180 |
+----+------------+-------+
2 rows in set (0.00 sec)

mysql> exit
Bye
bash-4.2$ exit
exit
[abonino@server1 ~]$
```
