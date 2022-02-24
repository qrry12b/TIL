# Docker Command

버전 정보를 출력하고 종료합니다
[REF](https://docs.docker.com/engine/reference/commandline/cli/)
```
docker -v
docker --version
```

버전 정보 표시 (모든 버전 정보)
[REF](https://docs.docker.com/engine/reference/commandline/version/)
```
docker version [OPTIONS]
```

이미지 목록을 조회합니다
[REF](https://docs.docker.com/engine/reference/commandline/images/)
```
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

Docker Hub에서 이미지를 받습니다
[REF](https://docs.docker.com/engine/reference/commandline/pull/)
```
docker pull <image name>
```

컨테이너 목록을 조회합니다 -a 인자를 추가하면 모든 컨테이너가 출력됩니다.
[REF](https://docs.docker.com/engine/reference/commandline/container_ls/)
```
docker container ls [OPTIONS]
docker container ls -a
```

이미지에서 새 컨테이너를 생성하고 실행합니다
[REF](https://docs.docker.com/engine/reference/commandline/container_run/)
```
docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

Docker 실행중인 컨테이너에 커멘드를 입력합니다
[REF](https://docs.docker.com/engine/reference/commandline/exec/)
```
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
docker exec -i -t <container> bash
docker exec -it <container> bash
```

중지된 컨테이너를 실행하거나 실행중인 컨테이너를 종료합니다
[REF](https://docs.docker.com/engine/reference/commandline/start/)
[REF](https://docs.docker.com/engine/reference/commandline/stop/)
```
docker start [OPTIONS] CONTAINER [CONTAINER...]
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```