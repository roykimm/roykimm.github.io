---
title: AWS 용량 늘리기
tags: aws
comments: true
---

출처 : https://leonkim.dev/aws/ce2-increase-disk-space/

### aws ec2용량 늘리기 ###
1. aws 콘솔 접속 후 ec2 서비스로 이동    
2. ebs 콘솔 화면으로 이동    
3. 해당 ebs을 선택 후 우클릭 하여 Modify Volume 을 선택 하기   
4. 원하는 디스크 사이즈를 입력 하고 Modify버튼을 클릭     
5. df -hT 명령어로 확인 , lsblk   
6. linux 파티션 크기 조정  
```
sudo growpart /dev/xvda 1
```
7. ex4 파일 시스템 확장
```
sudo resize2fs /dev/xvda1
```
8. df -hT 명령어로 실제 시스템이 확장 되었는지 확인.
