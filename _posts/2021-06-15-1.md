---
title: Docker 기본
tags: docker
comments: true
---

### docker 사용법 정리 ###

출처 : https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html


### 도커 설치 하기 ###

#### 자동 설치 스크립트로 설치 ####

```python
curl -fsSL https://get.docker.com/ | sudo sh
```

#### sudo 없이 사용하기 ####
root 가 아닌 사용자가 sudo없이 사용하려면 해당 사용자를 $docker$그룹에 추가 한다.

```
sudo usermod -aG docker $User # 현재 접속중인 사용자에게 권한 주기
sudo usermod -aG docker roykimm # roykimm 에게 권한주기
```

#### docker버전 확인하기 ####

```
docker version
```

#### 컨테이너 실행하기 ####
도커를 실행하는 명령어. 

```
docker run [option] Image
```
|옵션 | 설명 |
|:----|:----|
|-d | 백그라운드 모드|
|-p | 호스트와 컨테이너 포트를 연결(포워딩)|
|-v | 호스트와 컨테이너 디렉토리를 연결(마운트)|
|-e | 컨테이너 내에서 사용할 환경변수 설정|
|-name | 컨테이버 이름 설정|
|-rm | 프로세스 종료시 컨테이너 자동 제거|
|-it | -i, -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션|
|-link | 컨테이너 연결[컨테이너명:별칭]|


docker run ubuntu:16.04
: run 명령어를 사용하면 사용할 이미지가 저장되어 있는지 확인 하고   
없다면 다운로드(pull)을 한 후 컨테이너를 생성하고 시작 한다.

docker run --rm -it ubuntu:16.04 /bin/bash
: 컨테이너 내부에 들어가기 위해 bash쉘을 실행 하고 키보드 입력을 위해 -it옵션을 준다.
: 프로세스가 종료되면 컨테이너가 자동으로 삭제 되도록 --rm옵션도 추가

redis container   
: redis는 메모리 기반의 기능을 가진 스토리지 임. 6379 포트로 통신. telnet 명령어로 테스트 가능

```
docker run -d -p 1234:6379 redis
set mykey bella
get mykey
```
: -d 옵션을 주었기 때문에 실행하자 마자 쉘로 돌아옴 (백그라운드로 동작)   
: -p 옵션을 통해 1234 포트를 6379 포트로 연결됨   


윈도우 에서 텔넷 사용하기   
제어판 -> 프로그램 -> 프로그램 기능 -> window 기능 켜기/끄기 -> 텔넷 클라이언트   

mysql container   
```
docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7
mysql -h 127.0.0.1 -uroot 
```

### 도커 기본 명령어 ###

docker ps [OPTIONS] : ps 는 실행중인 컨테이너 목록을 보여줌
docker ps -a : 실행되었다가 종료된 컨테이너도 보여줌
docker stop [OPTIONS] container 

docker stop [컨테이너 아이디] : 앞부분만 입력 해도 됨
docker rm [OPTIONS] container 
docker rm [우분투 컨테이너 아이디]

### 이미지 목록 확인 하기 ###
docker images 

doker pull ubuntu:14.04 : 이미지 다운로드 하기(pull)

### 이미지 삭제 하기 ###
docker rmi [컨테이너 아이디]

### 로그 보기 ###
docker logs --tail 10 [컨테이너 아이디]

### 컨테이너 명령어 실행하기 ###

```
docker exec -it mysql /bin/bash
$ mysql -uroot
```

도커에서 컨테이너를 업데이트 하려면 새버전의 이미지를 다운 받고 기존 컨테이너를 삭제 한후   
새 이미지를 기반으로 새 컨테이버를 실행 하면됨.

컨테이너를 삭제 하면 컨테이버 에서 생성된 파일이 삭제된다는 말임.
데이터는 반드시 컨테이너 내부가 아닌 외부 스토리지에 저장 해야 한다.
aws s3같은 클라우드 또는 데이터 볼륨을 사용 하면 해당 디렉토리는 컨테니어와 별도로 저장 되고
컨테이너를 삭제 해도 데이터는 지워지지가 않음

```
docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql 
  -v /mybella/data:/var/lib/mysql
  mysql:5.7

```
/mybella/data 디렉토리에 저장 되고 컨테이너를 삭제 해도 데이터는 사라지지 않음   


### docker-compose 설치 ###

```
version: '2'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: wordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     volumes:
       - wp_data:/var/www/html
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_PASSWORD: wordpress
volumes:
    db_data:
    wp_data:

```
docker-compose.yml 파일 작성
docker-compose up   
