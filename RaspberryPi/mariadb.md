## mariaDB 설치
sudo apt-cache search mariadb-server   
sudo apt-get install mariadb-server -y   
mysql -V   

- - - - -
## mariaDB 접속 및 root 비밀번호 설정
sudo mysql -u root   
MariaDB[(none)]>> show global variables like 'port';   
use mysql   
select user, host, password from user;   
set PASSWORD for 'root'@'localhost'=PASSWORD('PASSWORD');   
flush privileges;   
exit   

sudo mysql -u root -p   

- - - - -
## 외부에서 접근하기 위한 설정
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf   
 > #bind-address = 127.0.0.1

sudo service mysql restart   
> 그 외 mysql user의 host가 localhost로 설정되어 있거나 방화벽 포트

- - - - -
## mariaDB 계정 추가
use mysql   
create user 'dev'@'%' identified by 'PASSWORD'   
flush privileges;

- - - - -
## 외부 IP에서 접근하는 계정에 DB 권한 부여 및 확인
GRANT DELETE, INSERT, SELECT, UPDATE ON DB_NAME.* TO dev@'%' IDENTIFIED BY 'PASSWORD';   
flush privileges;   
SHOW GRANTS FOR dev@'%';   

GRANT , REVOKE 에서 사용 가능한 권한은 다음 문서를 참고할 수 있습니다.   
https://dev.mysql.com/doc/refman/5.6/en/grant.html#grant-overview