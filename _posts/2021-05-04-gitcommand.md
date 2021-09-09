---
title: GIT 명령어 정리
tags: git
comments: true
---
GIT 이란?
git은 소스관리를 위한 분산 버전 관리 시스템. 리눅스 토발즈가 개발

git 설치 for mac 
1. https://git-scm.com/download/mac 사이트로 이동 후 installer 파일 다운로드
2. brew install git

```python
C:\Users\royki>git --version
git version 2.30.0.windows.1
```
설치 후 상단의 명령어를 입력하여 설치를 확인.

fork : 자신의 저장소에 복사후 clone 하는 것 
clone : 소스를 복사.

최신의 내용을 git에서 받으려면 
git pull  : 코드를 받아와 변경점을 merge한다.
git fetch : 코드를 받아 온다. 
※ 차이점 : merge를 하느냐 하지 않느냐의 차이.

git init : 새로운 저장소 만들기


Branch
: 나무가지 라는 뜻
: 만들어 놓은 버전(master)의 복사본(branch)을 만들어 다른 방향으로 작업을 이어 나가는것.

git branch : 현재 브랜치 목록보기
git branch '브랜치명' : 새로운 브랜치 생성하기
git checkout '브랜치이름' : 새로운 브랜치로 이동
git merge master '브랜치명' : 새로운 브랜치를 master와 병합한다.

git 코드상태 : uncommit, staying , commit 으로 나눈다. 
코드의 내용을 바꾸면 uncommit 상태
add 하면 staying 상태
commit 하면 확정 (서버에 push 가능상태)

git reset // add한 파일 취소
git reset --merge // merge한 파일 취소
git rest HEAD^ // 바로 직전에 커밋한 코드로 되돌아감.

코드를 잠시 저장후 다른 브랜치로 이동하기
git stash // 코드를 stash 영역에 잠시 저장하기
git stash pop // 마지막에 저장한 코드 불러오기
git stash list // stash 목록 확인하기





```python
C:\Users\royki>mkdir project
C:\Users\royki>cd project
C:\Users\royki\project>python -m venv venv
```

프로젝트 폴더내에서 가상환경명\Scripts\activate.bat을 입력하면 가상환경이 활성화 된다.


```python
C:\Users\royki\project>venv\Scripts\activate.bat  //활성화
C:\Users\royki\project>venv\Scripts\deactivate.bat  // 비활성화
```

기타 명령어 
pip freeze || pip list : 설치된 패키지 목록을 확인.

pip install [패키지명] : 해당 패키지를 install.

pip uninstall [패키지명] : 해당 패키지를 uninstall.

pip freeze > requirements.txt : 설치된 패키지 목록들을 requirements.txt 파일에 저장.

pip install -r requirements.txt : 파일에 저장된 패키지를 다른가상환경에 설치 한다.




