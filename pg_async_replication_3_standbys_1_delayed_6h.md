hpuser@hpuser-HP-Laptop-15-bs0xx:~$ ps -ef --forest | grep [p]ostgres
postgres    1473       1  0 07:04 ?        00:00:00 /usr/lib/postgresql/16/bin/postgres -D /var/lib/postgresql/16/main -c config_file=/etc/postgresql/16/main/postgresql.conf
postgres    1507    1473  0 07:04 ?        00:00:00  \_ postgres: 16/main: checkpointer 
postgres    1508    1473  0 07:04 ?        00:00:00  \_ postgres: 16/main: background writer 
postgres    1722    1473  0 07:04 ?        00:00:00  \_ postgres: 16/main: walwriter 
postgres    1723    1473  0 07:04 ?        00:00:00  \_ postgres: 16/main: autovacuum launcher 
postgres    1724    1473  0 07:04 ?        00:00:00  \_ postgres: 16/main: archiver last was 0000000100000002000000D4.00000028.backup
postgres    1725    1473  0 07:04 ?        00:00:00  \_ postgres: 16/main: logical replication launcher 
postgres   15332    1473  0 14:27 ?        00:00:00  \_ postgres: 16/main: walsender postgres 192.168.1.177(38744) streaming 2/D5000148
postgres   15333    1473  0 14:27 ?        00:00:00  \_ postgres: 16/main: walsender postgres 192.168.1.178(39444) streaming 2/D5000148
postgres   16002    1473  0 15:07 ?        00:00:00  \_ postgres: 16/main: walsender postgres 192.168.1.179(43788) streaming 2/D5000148


-------------------------------------------------------

  pid  | usesysid | usename  | application_name |  client_addr  | client_hostname | client_port |          backend_start           | backend_xmin |   state   |  sent_lsn  | write_lsn  | flush_lsn  | replay_lsn | write_lag | flush_lag | replay_lag | sync_priority | sync_state |            reply_time            
-------+----------+----------+------------------+---------------+-----------------+-------------+----------------------------------+--------------+-----------+------------+------------+------------+------------+-----------+-----------+------------+---------------+------------+----------------------------------
 16255 |       10 | postgres | node178          | 192.168.1.178 |                 |       57290 | 2026-11-24 15:29:59.759424+05:30 |              | streaming | 2/D5000148 | 2/D5000148 | 2/D5000148 | 2/D5000148 |           |           |            |             0 | async      | 2026-11-24 15:32:20.1522+05:30
 16256 |       10 | postgres | node177          | 192.168.1.177 |                 |       40128 | 2026-11-24 15:29:59.764053+05:30 |              | streaming | 2/D5000148 | 2/D5000148 | 2/D5000148 | 2/D5000148 |           |           |            |             0 | async      | 2026-11-24 15:32:20.187663+05:30
 16258 |       10 | postgres | node179          | 192.168.1.179 |                 |       37462 | 2026-11-24 15:30:00.377036+05:30 |              | streaming | 2/D5000148 | 2/D5000148 | 2/D5000148 | 2/D5000148 |           |           |            |             0 | async      | 2026-11-24 15:32:21.212601+05:30
(3 rows)


standbyuser@standby:~$ sudo -i -u postgres  psql -h /tmp
[sudo] password for standbyuser: 
psql (16.15 (Ubuntu 16.15-0ubuntu0.24.04.1))
Type "help" for help.

postgres=# show recovery_min_apply_delay;
 recovery_min_apply_delay 
--------------------------
 6h
(1 row)

postgres=# \q
could not save history to file "/var/lib/postgresql/.psql_history": No such file or directory
standbyuser@standby:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:f7:3b:61 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.179/24 brd 192.168.1.255 scope global enp0s3

---------------------------------------------------------------------------------------------------------------------------------





  pid  | usesysid | usename  | application_name |  client_addr  | client_hostname | client_port |          backend_start           | backend_xmin |   state   |  sent_lsn  | write_lsn  | flush_lsn  | replay_lsn |    write_lag    |    flush_lag    |   replay_lag    | sync_priority | sync_state |            reply_time            
-------+----------+----------+------------------+---------------+-----------------+-------------+----------------------------------+--------------+-----------+------------+------------+------------+------------+-----------------+-----------------+-----------------+---------------+------------+----------------------------------
 16255 |       10 | postgres | node178          | 192.168.1.178 |                 |       57290 | 2026-11-24 15:29:59.759424+05:30 |              | streaming | 2/D5E840B0 | 2/D5E840B0 | 2/D5E840B0 | 2/D5E840B0 |                 |                 |                 |             0 | async      | 2026-11-24 15:44:39.956986+05:30
 16256 |       10 | postgres | node177          | 192.168.1.177 |                 |       40128 | 2026-11-24 15:29:59.764053+05:30 |              | streaming | 2/D5E840B0 | 2/D5E840B0 | 2/D5E840B0 | 2/D5E840B0 |                 |                 |                 |             0 | async      | 2026-11-24 15:44:40.780281+05:30
 16258 |       10 | postgres | node179          | 192.168.1.179 |                 |       37462 | 2026-11-24 15:30:00.377036+05:30 |              | streaming | 2/D5E840B0 | 2/D5E840B0 | 2/D5E840B0 | 2/D5020670 | 00:00:00.016184 | 00:00:00.028005 | 00:10:29.094317 |             0 | async      | 2026-11-24 15:44:39.968049+05:30
(3 rows)

