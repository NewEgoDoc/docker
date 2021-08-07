# **컨테이너 사용**

## `1.컨테이너 이미지를 어떻게 사용해요?`
___

$docker pull 이미지이름:태그
$docker run 이미지이름:태그

컨테이너 이미지 검색 docker search 옵션 이미지이름:태그
이미지 다운로드docker pull 옵션 이미지이름:태그
다운 받은 이미지 목록 출력 docker images = docker image ls
다운 받은 이미지 상세보기 docker inspect 옵션 이미지이름:태그명
이미지 삭제 docker rmi

## `2.컨테이너를 실행하고 종료하는 명령을 알고 싶어요`
___

* 컨테이너 라이프 사이클

docker create --name webserver nginx:1.14 -> 컨테이너화 시킴

docker start webserver

pull -> create -> start

docker run --name webserver -d nginx:1.14

docker ps

docker ps -a

docker inspect webserver 
> 세부정보 확인: IPAddress 할당받은 정보 알수 있음

docker inspect --format '{{.NetworkSetting.IPAddress}}' webserver 

alias  단축어명 = "명령어" //더블쿼테이션 안에 있어야한다.

docker stop webserver

docker rm webserver

## `3.동작중인 컨테이너를 관리 명령어가 궁금해요`
___
* 실행중인 컨테이너 관리

docker ps

docker top webserver 컨테이너에서 실행 중인 프로세스 출력 

docker logs webserver

docker logs -f (실시간 모니터링 로그) webserver

docker exec -it webserver /bin/bash

docker stop webserver 잠시 중지

docker rm webserver  실행중인 컨테이너는 삭제 불가!

docker rm -f webserver

