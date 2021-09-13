---
title: 파이썬 가상환경 설정방법
tags: python
comments: true
---

파이썬 사용시 가상환경을 사용하는 이유?
1. 패키지 간의 의존성때문에 사용함. 
2. 작업환경이 바뀌더라도 동일한 환경을 재구성할수가 있음.

가상환경 종류는 venv(virtualenv) 와 pipenv가 있다.

venv 생성하는 방법
1. 바탕화면에 프로젝트를 생성한다. mkdir project
2. 생성한 폴더로 이동 cd project  
3. python -m venv venv[가상환경명]


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




