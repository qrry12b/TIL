# Dockerfile 

### FROM
베이스 이미지를 지정합니다

```
ex) FROM python:alpine3.15
```

### ENTRYPOINT

```
ex) ENTRYPOINT python manage.py runserver 0:8000
```

- - - - -

### WORKDIR

```
ex) WORKDIR /home/usr1/
```

- - - - -

### ADD

- - - - -

### COPY
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