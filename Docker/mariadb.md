# Docker에서 MariaDB 실행하기

```
docker -v
docker pull mariadb
docker container run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=MY_PASSWORD --name mariadb mariadb
docker container ls -a
docker exec -i -t mariadb bash
```

```
apt-get update
apt-get upgrade -y
apt-get install nano -y
apt-get install vim -y

nano /etc/mysql/mariadb.conf.d/50-server.cnf
# 만약 bind-address = 127.0.0.1 이 활성화 되어 있다면 외부IP 접근 불가
```

```
mysql -u root -p
/\# Enter password : MY_PASSWORD
show databases;