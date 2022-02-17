# 라즈베리파이에 Redis 설치하기   

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.10.12)   

wget http://download.redis.io/redis-stable.tar.gz

tar xvzf redis-stable.tar.gz

cd redis-stable

make

sudo make install

ps -ef | grep redis-server

redis-cli shutdown

redis-server &

sudo ls -l /proc/{PID}/exe

sudo mkdir /etc/redis

sudo mkdir /var/redis

sudo cp utils/redis_init_script /etc/init.d/redis_6379

sudo vi /etc/init.d/redis_6379

sudo cp redis.conf /etc/redis/6379.conf

sudo mkdir /var/redis/6379

sudo update-rc.d redis_6379 defaults

sudo /etc/init.d/redis_6379 start

### REF
* [redis.io #1](https://redis.io/topics/quickstart)
* [redis.io #2](https://redis.io/commands/auth)
* [tistory #1](https://info-lab.tistory.com/42)
* [tistory #2](https://smallgiant.tistory.com/71)
* [tistory #3](https://mansookim.tistory.com/90)
* [tistory #4](https://dgkim5360.tistory.com/entry/install-redis-for-linux-or-windows)
* [tistory #5](https://jeong-pro.tistory.com/139)
* [tistory #6](https://bcho.tistory.com/654)
* [qastack #1](https://qastack.kr/superuser/103309/how-can-i-know-the-absolute-path-of-a-running-process)