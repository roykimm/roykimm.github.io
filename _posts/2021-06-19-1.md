---
title: Ubuntu에 docker postgresql 설치 및 설정하기
tags: ubuntu docker aws
comments: true
---

### ubuntu에 docker postgresql 설치 및 설정하기 ###

#### 우분투 버전 확인 ####
cat /etc/issue 명령어로 확인하기   
Ubuntu 20.04.2 LTS \n \l    

#### docker 설치하기 ####
```
apt-get update  # 레포지토리 업데이트
apt-get install docker.io # 도커 설치

sudo su 
docker pull postgres # 온라인 레포지토리에서 도커 이미지를 다운
docker images # 시스템에 설치된 docker 이미지를 나열

# 도커이미지를 사용 하여 새 컨테이너 시작
docker run -d --name postgres-server -p 5432:5432 -e "POSTGRES_PASSWROD=1234" postgres  
```

#### docker PostgresSQL 영구데이터 작업 ####
```
docker volume create postgres-data  # 영구데이터에 대한 도커 볼륨을 만듬
docker volume inspect postgres-data # 영구데이터 디렉터리 확인하기

# 영구데이터 저장소를 사용하여 postgresql 컨테이너 시작
docker run -d --name postgres-server -p 5432:5432 -v postgres-data:/var/lib/postgresql/data -e "POSTGRES_PASSWORD=1234" postgres

ls /var/lib/docker/volumes/postgres-data/_data # 영구데이터 디렉터리 내용을 확인
```

#### docker 설치 테스트 ####
```
apt-get install postgresql-client

psql -h 127.0.0.1 -U postgres  # u 뒤에는 유저명임
alter user postgres with password '1234';

```

#### 유저 패스워드가 잘 안될때 ####
```
cd /var/lib/docker/volumes/postgres-data/_data
vi pg_hba.conf # method 를 md5 -> trust 로 수정 하면된다. 
```

#### 유저 생성 및 데이터 베이스 생성 ####
```
create user bella password 'bella' superuser createdb ;

select * from pg_user;

create database mydb owner roykimm;

select * from pg_database;

create table star (
	id integer not null,
	name character varying(255),
	class character varying(32),
	age integer,
	radius integer,
	lum integer,
	magnt integer,
	constraint star_pk primary key(id)
);

select *
  from star;
  
 
insert into star values('2', '금성', '지구',10,10,10,10)

```

#### aws 포트 추가 ####
aws 콘솔로 들어 간 후 security groups으로 들어가 inbound rule에 5432포트를 추가 한다.





