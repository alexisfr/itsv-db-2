## IDE

Multi-platform database tool [DBeaver](http://dbeaver.jkiss.org/)


## Mysql with Sakila DB
#### Prerequisites
- Docker https://docs.docker.com/engine/getstarted/linux_install_help/

```bash
$ sudo docker kill mysql
$ sudo docker rm mysql
$ sudo docker run --name mysql -e MYSQL_ROOT_PASSWORD=password -d -p 3306:3306 -v /home/user/sqkiladb:/usr/lib/mysql mysql:latest

$ sudo docker exec -it mysql mysql -uroot -p

mysql> CREATE USER 'user'@'%' IDENTIFIED BY 'password';
mysql> GRANT ALL ON *.* TO 'user'@'%';
mysql> quit

$ wget http://downloads.mysql.com/docs/sakila-db.tar.gz
$ tar -zxvf sakila-db.tar.gz

$ cd sakila-db

$ sudo docker exec -i mysql mysql -uuser -ppassword < sakila-schema.sql
$ sudo docker exec -i mysql mysql -uuser -ppassword < sakila-data.sql
```