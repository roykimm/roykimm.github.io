---
title: vscode 에서 emmet 적용이 안될때
tags: etc
comments: true
---

### vs code 에서 emmet이 적용 안될시 ###

1. ctrl + shift + p 클릭 하여 settings를 검색    
2. open settings UI를 클릭    
3. emmet 을 검색 한다.    
4. edit in settings.json을  클릭   
5. 하단의 코드를 넣어준다.   

```
  "emmet.triggerExpansionOnTab" : true,
  "files.associations": {
      "*html": "html"
  },
```
