---
title: React npm run build 시 멈춤현상
tags: react linux
comments: true
---

출처 : https://progdev.tistory.com/26   

AWS에서 react앱 build시 Creating an optimized production build 이후로 멈추는 현상이 나올시

이유 : aws 프리티어를 사용 하면 램이 1기가 밖에 되지 않기 때문에..   

#### 해결방법1 : #### 
빌드 스크립트를 하단 코드와 같이 변경한다.   
```
 ...
"build": "GENERATE_SOURCEMAP=false react-scripts build",
 ... 

```

#### 해결방법2 : 메모리 스왑을 통한 메모리 부족현상 처리 ####
```
free
sudo dd if=/dev/zero of=/mnt/swapfile bs=1M count=2048
sudo mkswap /mnt/swapfile
sudo swapon /mnt/swapfile


# 스왑메모리 해제 방법
sudo swapoff -v /mnt/swapfile
sudo rm /mnt/swapfile
```
