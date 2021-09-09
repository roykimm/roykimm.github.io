---
title: 리눅스 명령어 간단정리
tags: linux
comments: true
---

### 리눅스 명령어 ####

참고사이트
[https://nuninaya.tistory.com/158](https://nuninaya.tistory.com/158)

    $ sudo ss -ltp       # listen 상태인 TCP 프로세스 정보 출력 (매우유용)
    $ adduser roykimm    # 루트로 로그인 해서 이용자를 추가
    $ alias mv='mv -i'   # 자주쓰이는 명령어를 바꿈
    $ bg                 # 사용중인 프로그램의 수행을 백그라운드로
    $ bash               # bash쉘로 바꾼다.
    $ cat 3 1.txt 1.txt  # 1과 2의 파일을 연결하여 test라는 파일에 저장
    $ chfn               # 사용자의 finger정보를 바꾸는 명령어
    $ chfs               # 사용자의 shell을 바꾸는 명령어
    $ date               # 현재시간을 출력
    $ df                 # 파일시스템 구조와 용량보여줌
    $ du                 # 파일사이즈를 보여줌
    $ echo               # 아규먼트로 오는 문장이나 주소를 보여줌
    $ exit               # 프로세스 종료
    $ find               # 파일명을 찾고 그경로를 출력
    $ finger roykimm     # 사용자정보
    $ free               # 메모리 사용상태와 남은상태등을 보여줌
    $ fsck               # 파일시스템 체크
    $ grep               # 파일중에서 어떠한 패턴을 검색
    $ groupadd           # 그룹이용자를 추가하는 명령어
    $ head               # 파일의 처음 n개의 내용을 보여줌
    $ history 5          # 작업한 내역을 5 개보여줌
    $ id                 # 사용자의 로그인명을 보여줌
    $ join               # 두개의 파일에서 공통된 라인을 찾아내는 명령어
    $ kill               # 실행되어 있는 프로세서 id를 입력해서 종료
    $ last               # 마지막으로 접근한 사용자 표시
    $ ln -s /usr/bella/ $home/doc # 앞의 파일, 디렉토리를 뒤로 링크시킴
    $ locate             # 저장위치를 찾아줌
    $ login              # 로그인
    $ ls -al             # 디렉토리와 파일정보를 출력
    $ mv test1 test2     # 파일의 이름을 변경하거나 이동하는 명령어
    $ passwd             # 사용자의 패스워드를 변경하는 명령어
    $ ping roykimm.ml    # 네트워크 통신 점검 명령어
    # password           # 프로세서에 관한 리스트를 보여줌
    # pwd                # 현재위치 보여줌
    # rm                 # 파일을 제거한다
    # rmdir              # 폴더를 제거한다
    # sh                 # 본셀을 불러내는 명령어
    # su                 # 사용중 다른아이디로 재접속
    # tar
    # tail               # 마지막 내용을 볼때사용
    # touch              # 파일 만들기
    # uptime             # 서버의 부팅시간과 현재시간 출력
    # vi                 # vi 에디터
    # who
    # whoami
