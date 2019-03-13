## IDE

Multi-platform database tool [DBeaver](http://dbeaver.jkiss.org/)


## Mysql with Sakila DB
#### Prerequisites
- Docker https://docs.docker.com/install/

```bash
mkdir ~/.mysql
docker run --name mysql -e MYSQL_ROOT_PASSWORD=password -d -p 3306:3306 -v ~/.mysql:/var/lib/mysql mysql:5.7

docker exec -it mysql mysql -uroot -p

mysql> CREATE USER 'user'@'%' IDENTIFIED BY 'password';
mysql> GRANT ALL ON *.* TO 'user'@'%';
mysql> quit

wget http://downloads.mysql.com/docs/sakila-db.tar.gz
tar -zxvf sakila-db.tar.gz

docker exec -i mysql mysql -uuser -ppassword < sakila-db/sakila-schema.sql
docker exec -i mysql mysql -uuser -ppassword < sakila-db/sakila-data.sql
```