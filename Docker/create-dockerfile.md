# Dockerfile 

### FROM
베이스 이미지를 지정합니다

```
ex) FROM python:alpine3.15
```

### ENTRYPOINT
컨테이너가 시작시 실행될 명령을 정의합니다<br/>
ENTRYPOINT 혹은 CMD가 선언된 경우 작업이 완료 시 컨테이너가 종료되며<br/>
컨테이너에 대해 stop 명령 사용시 종료 관련 SIGNAL 을 보냅니다 (SIGNTERM, SIGKILL)

```
ex) ENTRYPOINT python manage.py runserver 0:8000
```

- - - - -

### WORKDIR
기본 디렉토리를 변경합니다

```
ex) WORKDIR /home/usr1/
```

- - - - -

### ADD

- - - - -

### COPY
파일을 복사합니다

```
ex) COPY ./config/ /home/usr1/config/
```

- - - - -

### ENV

- - - - -

### RUN

```
ex) RUN init_.sh
```

- - - - -

### EXPOSE

```
ex) EXPOSE 8000
```

- - - - -

### LABEL

- - - - -

### STOPSIGNAL

- - - - -

### CMD

- - - - -

### USER

- - - - -

### VOLUME

- - - - -

### ONBUILD

- - - - -

[REF #1](https://docs.docker.com/engine/reference/builder/)
[REF #2](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)