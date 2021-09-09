---
title: 파이썬 설치 for windows
tags: python
comments: true
---

1. 파이썬 공식 홈페이지 접속 www.python.org

2. download windows x86-64 executable installer 클릭하여 인스톨러 파일 내려받기

3. 인스톨러 프로그램이 실행되면 add python 3.8 to PATH 체크박스에 체크 하기
  -> 체크해야 윈도우 PATH환경변수에 Python38 디렉터리와 Scripts 디렉터리를 자동으로 추가해줌

4. 시스템 환경변수 편집
  -> 체크박스에 체크 하지 않았다면 윈도우 검색메뉴에서 시스템 환경변수 편집 검색
  -> 환경변수 편집 창에서 path 환경변수에 Python38 디렉터리와 Scripts 디렉터리 추가
  -> 내경우는 C:\Users\royki\AppData\Local\Programs\Python\Python38
  -> C:\Users\royki\AppData\Local\Programs\Python\Python38\Scripts\
  
5. 설치확인
  -> cmd 창에 set path를 쳐서 디렉터리 경로 확인
  -> python --version 을 입력 하여 버전 확인하기