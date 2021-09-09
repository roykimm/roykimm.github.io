---
title: django react 프로젝트 aws에 배포시 체크사항
tags: django react nginx ubuntu github
comments: true
---

# django react 프로젝트 배포

참고사이트
[velog.io](https://velog.io/@du-du-zi/Django-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%EC%84%9C%EB%B2%84-%EB%B0%B0%ED%8F%ACnginxuwsgigit-%ED%99%9C%EC%9A%A9)
[killu.tistory.com](https://killu.tistory.com/66)
[nachwon.github.io](https://nachwon.github.io/django-deploy-2-wsgi/)
[webdir.tistory.com](https://webdir.tistory.com/241)
[2atom.tistory.com](https://2atom.tistory.com/50)

개발기술
backend : python / django
frontend : react / material-ui / react-router

서버
aws - Ubuntu
nginx

기타
github

# settings.py 설정

배포에 앞서서 settings.py 에서 설정을 바꾸어준다.
수정할 내용은 정적파일을 모으기 위해서 이다.
css 파일등과 같은 정적 파일들은 장고내의 여러 폴더에 분산되어 있기 때문에
사용하는 모든 정적 파일을 하나의 경로로 모아주는 작업이 필요하다.
이때 사용하는 것이 collectstatic 명령어이다.

    from pathlib import Path
    import os

    BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    ROOT_DIR = os.path.dirname(BASE_DIR)
    STATIC_URL = '/static/'
    STATIC_DIR = os.path.join(BASE_DIR, 'static')
    STATICFILES_DIRS = [
        STATIC_DIR,
    ]
    STATIC_ROOT = os.path.join(ROOT_DIR, '.static_root')

    ALLOWED_HOSTS = '*'

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }

프로젝트 루트의 상위 폴더에 .static_root 라는 숨김폴더를 생성 하고 그곳으로
모든 정적파일을 모으도록 설정.

# 파이썬 패키지 저장

현재 사용되었던 파이썬 패키지를 파일로 저장한다.
서버에서 venv를 사용하여 동일한 파이썬 패키지를 구성하기 위해서 사용한다.
터미널에서 하단의 명령어를 입력 하면 requirements.txt라는 파일이 생성된다.

    # pip freeze > requirements.txt

# github 에 프로젝트 올리기

우선 모든 로컬작업 파일을 github에 올리자.

    # github에 접속 하여 git 저장소 만들기
    git init
    # .gitignore 파일 만든 후 작성
    git add .gitignore
    git commit -m "first commit"
    git remote add origin https://github.com/roykimm/test.git
    git push -u origin master

    # 최초로 올린후 나머지 파일 업로드
    git add .
    git commit -m "second commit"
    git push origin master

# .gitignore파일

이 파일에는 git에 올리지 않을 파일이나 폴더명을 적어준다.

    ### React ###
    node_modules
    package-lock.json
    music_controller/frontend/static/frontend/  # 이거 잘안되던데 나중에 확인
    #music_controller/frontend/static/frontend/main.js
    #music_controller/frontend/static/frontend/main.js.LICENSE.txt
    #music_controller/frontend/static/frontend/main.js.map
    ### Django ###
    *.log
    *.pot
    *.pyc
    __pycache__/
    local_settings.py
    db.sqlite3
    db.sqlite3-journal
    .static_root/           # 개발에서는 runserver를 사용하므로 사용안함.
    media

# 서버 관련 준비

1. apt-get 최신버전 업데이트 하기

   sudo apt-get update
   sudo apt-get Upgrade

2. 파이썬 설치 하기
3. pip 설치 하기

   apt-get install python3-pip

# AWS EC2서버에서 git clone 하기

git clone 은 git서버에 올려놓은 프로젝트를 복제하는 명령어이다.
우선 서버에 접속을 한다.

    $ ssh -i ~/.ssh/mykey.pem ubuntu@ec2-18-222-188-155.us-east-2.compute.amazonaws.com

프로젝트는 /srv/ 폴더에 다운로드 할것이다.
폴더는 chown 명령어로 소유권을 바꾸어 준다.

    $ chown -R deploy:deploy /srv/

이제 /srv/폴더로 이동 하여 github에 올라가 있는 프로젝트를 clone 해준다.

    # cd /srv
    # git clone https://github.com/roykimm/[프로젝트명].git
    # ls        # ls 명령어로 확인 하기

# 가상환경 관련 설정

    python -m venv venv     # 나는 /srv/music_controller 의 위치에서 실행
    source venv/bin/activate # venv활성화
    # 비활성화 시키고 싶을 경우는 deactivate
    pip install -r requirements.txt # 개발환경에서 사용된 python패키지 설치

# python 마이그레이션 & collectstatic

manage.py 파일이 있는 경로로 이동 하여 하단의 명령어를 입력

    python manage.py makemigrations
    python manage.py migrate
    python manage.py collectstatic

# 리액트 npm 설치 및 build 하기

node_module이 설치 되지 않았기 때문에 모듈을 설치 해주어야 한다.
그리고 개발과는 다르게 build명령어로 리액트 컴포넌트들을 build하여 준다.

    cd /srv/music_controller/frontend
    npm install     # package.json 파일이 있는 경로에서
    npm run build   # webpack에 설정한대로 ./static/frontend/main.js로 빌드된다.

# uwsgi 서버 연결하기

wsgi란 ? webServer와 Python으로 작성된 webApplication또는 framework들간의 인터페이스를 정의해 놓은 규칙이다.
django는 웹서버와 직접 통신할수 없기 때문에 둘 사이에 uwsgi를 두어야 한다.

구조 : client <-> Nginx Server <-> uwsgi
staic은 nginx 가 핸들링 하고 나머지 request에 대해서는 reverse proxying을 통해
uwsgi에게 전달하여 사용하게 한다.

    pip install uwsgi # venv 활성화 상태에서 실행한다.

설정파일을 작성한다. manage.py파일이 있는 경로에서 vi uwsgi.ini 로 실행하여 하단내용을 작성한다.

    [uwsgi]
    chdir = /srv/music_controller/music_controller # 실제 프로젝트 시작 폴더
    module = music_controller.wsgi:application     # settings.py 에 WSGI_APPLICATION 와 동일하게
    home = /srv/music_controller/venv              # venv 가 있는 경로

    uid = deploy
    gid = deploy

    #http = :8080

    socket = /tmp/music_controller.sock
    chmod-socket = 666
    chown-socket = deploy:deploy

    enable-threads = true
    master = true
    vacuum = true
    pidfile = /tmp/music_controller.pid
    logto = /var/log/uwsgi/music_controller/@(exec://date +%%Y-%%m-%%d).log
    log-reason = true

service 파일을 작성한다. manage.py파일이 있는 경로에서 vi uwsgi.service
로 실행하여 하단내용을 작성한다.

    Description=uWSGI service
    After=syslog.target

    [Service]
    ExecStart=/srv/music_controller/venv/bin/uwsgi -i /srv/music_controller/music_controller/uwsgi.ini

    Restart=always
    KillSignal=SIGQUIT
    Type=notify
    StandardError=syslog
    NotifyAccess=all

    [Install]
    WantedBy=multi-user.target

위 service 파일을 만든 후에 uwsgi.sevice파일을 /etc/systemd/system/ 에 하드링크를 걸어준다.

    sudo ln -f /srv/music_controller/music_controller/uwsgi.service /etc/systemd/system/uwsgi.service
    # systemd 는 리눅스 프로세스를 관리하는 init 시스템이며
    # systemd 의 d는 daemon을 의미

위의 데몬으로 등록 하면 서버가 시작할때 자동으로 백그라운드에서 실행될수 있도록 할수 있음.

    sudo systemctl daemon-reload  # 데몬 리로드
    sudo systemctl enable uwsgi # uwsgi로 데몬을 활성화

    sudo systemctl | grep nginx # 확인
    sudo systemctl | grep uwsgi # 확인

# nginx 설치 및 설정

    # PPA 추가를 위한 필요 패키지
    sudo apt-get install software-properties-common python-software-properties
    # nginx 안정화 최신버전 PPA 추가
    sudo add-apt-repository ppa:nginx/stable
    # PPA 저장소 업데이트
    sudo apt-get update
    # nginx 설치
    sudo apt-get install nginx

    nginx -v

배포관련 작업은 deploy 유저가 담당 하기 때문에 nginx유저를 deploy로 바꿔준다.

    sudo vi /etc/nginx/nginx.conf
    user www-data 를 user deploy로 수정하여 준다

mysite.conf파일을 만든다.

    cd /etc/nginx/sites-available/
    vi mysite.conf

mysite.conf파일에 하단의 코드를 넣는다. (내가 사용하는 파일)

    server {
        listen 80;
        server_name ec2-18-222-12323.us-east-2.compute.amazonaws.com;
        charset utf-8;
        client_max_body_size 128M;

        location / {
            uwsgi_pass unix://tmp/mysite.sock;
            include uwsgi_params;
        }

        location /static/ {
            alias /srv/django-deploy/static/;
        }
    }

    server {
        listen 3000;
        server_name ec2-18-222-188-155.us-east-2.compute.amazonaws.com;
        charset utf-8;

        location / {
            root /srv/my-react/build;
            index index.html index.htm;
            try_files $uri $uri/ /index.html;
        }
    }

    server {
        listen 7000;
        server_name ec2-18-222-188-155.us-east-2.compute.amazonaws.com;
        charset utf-8;
        client_max_body_size 128M;

        location / {
            uwsgi_pass unix://tmp/music_controller.sock;
            include uwsgi_params;
        }

        location /static/ {
            alias /srv/music_controller/.static_root/;
        }
    }

작성이 완료 되었으면 다음의 명령어를 입력하여
sites-available 에 있는 설정 파일을 sites-enabled 폴더에 링크 해준다.

    sudo ln -sf /etc/nginx/sites-available/mysite.conf /etc/nginx/sites-enabled/mysite.conf

작업이 완료 되면 올리고 나면 서버에서 데몬 리로드, nginx와 uWSGI를 재부팅해준다.

    sudo systemctl daemon-reload
    sudo systemctl restart nginx uwsgi

# AWS 인바운드 규칙 편집

    인바운드 규칙에 사용 포트를 열어 준다.
    나의 경우는 80, 3000, 7000번 포트를 열어준다.

# 서버 에러 발생시

하단의 로그 파일 에서 확인이 가능하다.

    tail -50 /var/log/uwsgi/music_controller/[로그작성일자].log
    tail -50 /var/log/nginx/error.log

# 업로드 파일 권한 주기

권한 때문에 db가 읽히지 않는 현상이 발생되었다.
하단의 명령어로 폴더 내에 사용자를 일괄 변경하자.

    sudo chown -R deploy:deploy music_controller/

# 계속적인 수정시 필요한 내용 정리

1. 서버접속
2. 해당 프로젝트로 이동 cd /srv/music_controller/
3. git pull origin master
4. frontend 로 이동하여 npm install && npm run build
5. python manage.py makemigrations & python manage.py migrate
6. python manage.py collectstatic
7. sudo systemctl restart nginx uwsgi
