---
title: django 프로젝트 aws에 배포
tags: python django
comments: true
---

### django 프로젝트 배포 ###
```
apt-get update
apt-get install build-essential
sudo apt-get install python3


ssh-keygen -t rsa
cat /home/ubuntu/.ssh/id_rsa.pub
키복사
github -> 프로젝트 들어가서 -> deploy key 에 추가 하기

git clone~~
python3 -m venv venv
activate 
pip install -r requirements.txt

allowhost 설정 후
python manage.py runserver 0.0.0.0:8002

웹서버와 연결 시켜줄 통신 (인터페이스 설정) uwsgi 

pip install uwsgi 
vi uwsgi.ini -- manage.py 가 있는 폴더에서

/home/ubuntu/dj/mytest

[uwsgi]
chdir=/home/ubuntu/dj/mytest
module=mytest.wsgi:application
master=True
pidfile=/tmp/project-master.pid
vacuum=True
max-requests=5000
daemonize=/home/ubuntu/dj/django.log
home=/home/ubuntu/dj/venv
virtualenv=/home/ubuntu/dj/venv
socket=/home/ubuntu/dj/uwsgi.sock
chmod-socket=666

uwsgi --ini uwsgi.ini

sudo apt-get install nginx
sudo vi /etc/nginx/nginx.conf
# user www-data;
user deploy;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {
        upstream django {
           server unix:/home/ubuntu/dj/uwsgi.sock;
        }
        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;

sudo vi /etc/nginx/sites-enabled/

vi /etc/nginx/sites-available
sudo cp default dj.conf
vi dj.conf

 root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                include /etc/nginx/uwsgi_params;
                uwsgi_pass django;
        }

        location /static/ {
                alias /home/ubuntu/dj/mytest/static/;
        }

        # pass PHP scripts to FastCGI server
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;

sudo ln -sf /etc/nginx/sites-available/dj.conf /etc/nginx/sites-enabled/dj.conf

sudo service nginx restart

```