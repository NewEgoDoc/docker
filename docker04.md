# **Docker Registry**

## `1.Docker Registry???`
___

1. Registry: 컨테이너 이미지를 저장하는 저장소, 

여러 형태의 컨테이너 이미지가 모이는 장소이다. 크게 두가지 레지스트리가 있고 그것은

> * public Registry인 `Docker hub`이며 
> * private Registry인 도커에서 제공하는 `Registry (image_)`이다.

2. Docker Hub: hub.docker.com
3. Private Registry: 사내의 컨테이너 저장소


## `2.Docker hub 사용`
___
* https://hub.docker.com/
* image 종류 : Official Images, Verified Publisher, etc.
* 이미지 검색
```shell
$docker search "targetImageName"
```
>결과
```
[root@localhost ~]# docker search nginx
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                             Official build of Nginx.                        15237     [OK]
jwilder/nginx-proxy               Automated Nginx reverse proxy for docker con…   2053                 [OK]
richarvey/nginx-php-fpm           Container running Nginx + PHP-FPM capable of…   816                  [OK]
jc21/nginx-proxy-manager          Docker container for managing Nginx proxy ho…   225
linuxserver/nginx                 An Nginx container, brought to you by LinuxS…   150
tiangolo/nginx-rtmp               Docker image with Nginx using the nginx-rtmp…   137                  [OK]
jlesage/nginx-proxy-manager       Docker container for Nginx Proxy Manager        126                  [OK]
alfg/nginx-rtmp                   NGINX, nginx-rtmp-module and FFmpeg from sou…   102                  [OK]
...
```

DockerHub에서 이미지 다운로드를 하기 위해선 `docker pull`을 사용한다.
```shell
$docker pull nginx:latest
```
>결과
```

...
```

DockerHub에서 다운로드 받은 이미지들을 확인하기 위해선 `docker images`을 사용한다.
```shell
$docker images
$docker image ls

$docker ps -a -q 
해당 내용은 도커 이미지들을 한번에 삭제 할때도 사용하니 알아두자
이 명령어는 모든 image의 UUID들만을 추출해낸다.
```
>결과
```

...
```

DockerHub에 접근하여 이미지들을 업로드하고 다운로드 받기 위해서는 `docker login`으로 해당 서버에 접근 권한을 가져와야한다.
해당 
```shell
$docker login

```
>결과
```
[root@localhost ~]# docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: 해당아이디
Password: 비밀번호는 보이지 않고 입력되니 주의하자
(틀리면 다시해야한다 ㅡㅡ)

...

WARNING! Your password will be stored unencrypted in /root/.docker/config.json
Configure a credential helper to remove this waring. See https://docs.docker.com/engine/reference/commandline/login/#credentials-stroe

Login Succeeded
root@root:~#

위 내용은 json의 형태로 리눅스 체제 아래에 해당 아이디와 비밀번호를 남겨 로그인 된 상태를 유지 한다는 소리이다.
당연히 로그인이 되었을 때 나오는 문구이다.
```
이제 어떤 공간에서든 우리가 만든 도커 컨테이너를 사용하고 싶다면 DockerHub에 업로드하려면 `docker push`으로 컨테이너 이미지를 올려주면 된다.

일단 올려주기 위해서 해당 tag 명을 살짝쿵 교체해주어야 한다.

`로그인아이디/컨테이너이름:태그`의 형식을 따라주어야 매칭되어 docker hub에 업로드가 가능하다.

```shell
$docker images 
   >해당 이미지를 검색하고

$docker tag webs:latest 로그인아이디/webs:latest
   >검색된 이미지의 태그명을 교체해준다

$docker push 로그인아이디/webs:latest
   >dockerhub에 이미지 업로드한다.
```
이렇게 푸시된 퍼블릭 컨테이너들은 누구든지 어디서든지 다운로드가 가능하다.

물론 원한다면 프라이빗 컨테이너 공간을 만들수 있지만 도커 허브 계정에서는 1개 까지 가능하고 *2개이상은 유료*화 되어있다.

## `3. Private Registry 구축`
___
* Registry 컨테이너를 이용해 Private 컨테이너 운영
```
docker run -d -p 5000:5000 --restart always --name registry registry:2
```
컨테이너가 동작을 하게 되면서 개인적으로 운영 할 수 있는 프라이빗 Registry가 만들어 지게 된다.

* image repository
```shell
$docker pull centos7
$docker tag centos7 localhost:5000/centos:tag
$docker push localhost:5000/centos:tag
```
호스트 네임:포트넘버/레파지토리 이름:태그
해당 형식을 적용해주어야 Private Registry에 업로드 가능하다.

컨테이너 이름 태그 까지만 있는 public registry와 달리 private registry는 앞에 반드시 호스트네임과 포트넘버가 들어가야된다. 포트넘버가 80번인 경우는 생략이 가능하다.
