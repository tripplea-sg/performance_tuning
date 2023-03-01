# Thread Pool
Install sysbench
```
sudo yum install sysbench

mysql -uroot -h::1 -e "alter user root@'localhost' identified with mysql_native_password by 'root'"
mysql -uroot -h::1 -proot -e "set persist max_connections=3000"
```
Run sysbench prepare
```
sysbench oltp_read_write --table_size=100000 --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root prepare
```
Sysbench run
```
sysbench oltp_read_write --threads=900 --time=60 --table_size=100000 --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root run
```
Sysbench cleanup
```
sysbench oltp_read_write --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root cleanup
```
Install Thread Pool
```
# add this to my.cnf (vi $HOME/mysql-sandboxes/3306/my.cnf)
plugin-load-add=thread_pool.so
thread_pool_size=8
thread_pool_stall_limit=200
```
restart mysql
```
mysql -uroot -h::1 -proot -e "restart"
mysql -uroot -h::1 -proot -e "set persist max_connections=3000"
```
Run sysbench prepare
```
sysbench oltp_read_write --table_size=100000 --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root prepare
```
Sysbench run
```
sysbench oltp_read_write --threads=900 --time=60 --table_size=100000 --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root run
```
Sysbench cleanup
```
sysbench oltp_read_write --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root cleanup
```
Tuning Thread Pool
```
mysql -uroot -h::1 -proot -e "set global thread_pool_query_threads_per_group=2"
```
Run sysbench prepare
```
sysbench oltp_read_write --table_size=100000 --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root prepare
```
Sysbench run
```
sysbench oltp_read_write --threads=900 --time=60 --table_size=100000 --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root run
```
Sysbench cleanup
```
sysbench oltp_read_write --mysql-db=airportdb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=root --mysql-password=root cleanup
```

