# Tuning Load Data
Download airport-db
```
wget https://downloads.mysql.com/docs/airport-db.tar.gz

tar -zxvf airport-db.tar.gz
```
Load airport-db to database and check the time
```
mysql -uroot -h::1 -e "set global local_infile=on"

time mysqlsh root@localhost:3306 -- util loadDump 'airport-db' 
```
Drop airport-db and restart database
```
mysql -uroot -h::1 -e "drop database airport-db"
mysql -uroot -h::1 -e "restart"
```
