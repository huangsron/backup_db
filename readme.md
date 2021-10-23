# mysql backup

## run
```shell
# init
docker-compose up -d

# enter container
docker-compose exec mysql bash

# import database
cd /tmp/sql/test_db/
mysql -u root -pdbadmin1234 < employees.sql
exit

# execute backup
docker-compose exec mysql sh -c "chmod 755 /tmp/sql/backup/backup.sh; /tmp/sql/backup/backup.sh"

sudo ls sql/db_backup/mysql

# stop container
docker-compose down
```

## clean
```shell
sudo git clean -f -X -d
```

## add test_db to project
```shell
git submodule add https://github.com/datacharmer/test_db.git sql/test_db
git submodule init
git submodule update --recursive
```

## note
```shell
docker-compose up -d

docker-compose exec mysql bash

cd /tmp/sql/test_db/
mysql -u root -pdbadmin1234 < employees.sql

mysql -u root -p
use employees;
select * from employees limit 10;

mysqldump -u root -p employees > /tmp/sql/backup/backup.sql

# restore database
mysql -u root -p
CREATE DATABASE `employees2`;
mysql -u root -p  employees2 < /tmp/sql/backup/backup.sql

mysql -u root -pdbadmin1234 -e "show databases;"

docker-compose down

# install mysql client
sudo apt install mysql-client

cat << EOF > .my.cnf
[client]
user=root
password=dbadmin1234
EOF

cat  .my.cnf

mysql --defaults-file=.my.cnf -h 127.0.0.1 -u root -pdbadmin1234 -e "show databases;"

# test backup
# docker-compose exec mysql sh -c "chown root:root /tmp/sql/backup/backup.sh; chmod 755 /tmp/sql/backup/backup.sh; /tmp/sql/backup/backup.sh"
docker-compose exec mysql sh -c "chmod 755 /tmp/sql/backup/backup.sh; /tmp/sql/backup/backup.sh"

sudo ls sql/db_backup/mysql
```

