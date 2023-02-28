# Performance Tuning
Set I/O scheduler
```
cat /sys/block/sda/queue/scheduler
sudo su
echo 'none' > /sys/block/sda/queue/scheduler
cat /sys/block/sda/queue/scheduler
exit
```
Create environment
```
sudo systemctl stop mysqld
sudo systemctl disable mysqld

mysqlsh -e "dba.deploySandboxInstance(3306)"
```
Download airport-db
```
wget https://downloads.mysql.com/docs/airport-db.tar.gz

tar -zxvf airport-db.tar.gz
```
Load airport-db to database and note the time
```
mysql -uroot -h::1 -e "set global local_infile=on"

time mysqlsh root@localhost:3306 -- util loadDump 'airport-db' 
```
Drop airport-db and restart database
```
mysql -uroot -h::1 -e "drop database airport-db"
mysql -uroot -h::1 -e "restart"
```
Set database parameters
```
mysql -uroot -h::1 << EOF
set persist innodb_buffer_pool_dump_at_shutdown=off;
set persist_only innodb_read_io_threads=16;
set persist_only innodb_write_io_threads=4;
set persist innodb_redo_log_capacity=12884901888;
set persist innodb_flush_neighbors=2;
set persist innodb_io_capacity=3000;
set persist innodb_io_capacity_max=6000;
set persist innodb_checksum_algorithm='strict_crc32';
set persist binlog_row_image=minimal;
set persist innodb_log_compressed_pages=off;
EOF

echo "innodb_buffer_pool_load_at_startup=off" >> /home/opc/mysql-sandboxes/3306/my.cnf

mysql -uroot -h::1 -e "restart"

mysql -uroot -h::1 << EOF
show variables like 'innodb_buffer_pool_dump_at_shutdown';
show variables like 'innodb_buffer_pool_load_at_startup';
show variables like 'innodb_read_io_threads';
show variables like 'innodb_write_io_threads';
show variables like 'innodb_redo_log_capacity';
show variables like 'innodb_flush_neighbors';
show variables like 'innodb_io_capacity';
show variables like 'innodb_io_capacity_max';
show variables like 'innodb_checksum_algorithm';
show variables like 'binlog_row_image';
show variables like 'innodb_flush_method';
show variables like 'innodb_log_compressed_pages';
EOF
```
In production, do not change innodb_buffer_pool_dump_at_shutdown and innodb_buffer_pool_load_at_startup </br>
Load airport-db to database and note the time
```
mysql -uroot -h::1 -e "set global local_infile=on"

time mysqlsh root@localhost:3306 -- util loadDump 'airport-db' 
```
Drop airport-db and restart database
```
mysql -uroot -h::1 -e "drop database airport-db"
mysql -uroot -h::1 -e "restart"
```
