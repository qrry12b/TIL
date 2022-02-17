# 라즈베리파이에 MariaDB 설치하기   

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.10.12)   

- - - - -

### MariaDB 설치하기   

터미널을 열고 순서대로 진행   

sudo apt-get update 와 sudo apt-get upgrade 로 패키지를 업데이트   
sudo apt-get install mariadb-server 로 마리아DB를 설치   

Do you want continue? [Y/n]라며 물어보면 Y를 입력   
설치가 끝나면 mariadb를 사용할 수 있다

- - - - -

### MariaDB 접속하기   

sudo mysql -u root mysql; 을 입력해 루트 계정으로 mysql 데이터베이스에 접속   
mariaDB의 mysql 데이터베이스에 접속했다면 가장 먼저 보안을 위해 루트 계정 비밀번호를 변경   

update user set password=password('루트비밀번호') where user='root';   
FLUSH PRIVILEGES;   

select host,user,password from user; 로 비밀번호가 설정되었는지 확인 할 수 있다   

- - - - -

### 데이터베이스와 사용자 추가

MariaDB에 데이터베이스와 사용자를 추가하고 사용자에게 데이터베이스에 접근할 수 있는 권한을 부여   

먼저 testdb 라는 이름의 데이터베이스를 생성. (데이터 베이스 이름은 다른 이름을 사용해도 상관없음)

create database testdb default character set utf8;   

데이터베이스를 생성했다면 이번에는 testus 라는 이름의 사용자를 생성. (동일하게 사용자 이름도 다른 이름을 사용할 수 있음)

create user 'testus'@'localhost' identified by '비밀번호';   

여기서 @ 앞에는 사용자 이름 @ 뒤에는 접속 가능한 host   
모든 host에서 접속 가능하게 하려면 @'localhost' 대신 @'%' 를 사용   

사용자와 데이터베이스 모두 생성했다면 마지막으로 사용자에게 데이터베이스에 작업할 권한을 부여   

grant all privileges on testdb.* to 'user1'@'%';   
FLUSH PRIVILEGES;   

여기서 testdb 는 데이터베이스명, *은 모든 테이블을 의미하며 뒤에 user1 은 사용자명으로 다른 이름으로 생성했다면 다른 이름으로 수정해 입력   

권한 부여 이후에는 sudo mysql -u 사용자명 -p 데이터베이스명 으로 접속   

- - - - -

### MariaDB 외부 접속 허용하기   

sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf   

bind-address 를 찾아서 맨 앞에 # 을 추가해 주석처리하고 저장   

sudo service mysql restart   

mysql(MariaDB) 를 재시작   

- - - - -

### MariaDB iptables 방화벽 설정하기

sudo iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
sudo iptables -A OUTPUT -p tcp --dport 3306 -j ACCEPT
sudo iptables-save

- - - - -

### 기타 MariaDB 쿼리

mysql 데이터베이스에서 SELECT 쿼리로 등록된 사용자를 조회   
```
select host, user , password from user;
```

등록된 사용자를 제거   
```
drop user '사용자명'@'호스트';
```

특정 사용자에게 부여헀던 데이터베이스 권한을 해제   
```
revoke all on 데이터베이스명.* from '사용자명';
```

생성된 데이터베이스를 조회   
```
show databases;
```

생성된 데이터베이스를 제거   
```
drop database 데이터베이스명;
```

사용할 데이터베이스를 지정   
```
use 데이터베이스명
```