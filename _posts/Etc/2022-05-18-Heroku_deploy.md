---
title: "[Heroku] Heroku를 통한 Deploy중 겪은 오류 해결"
categories:
  - Etc
tags:
  - ETC
  - Heroku

toc: true
toc_sticky: true

date: 2022-05-18
last_modified_at: 2022-05-18
---
<br>

최근 진행하는 토이프로젝트를 배포하려고 여러 방법을 찾아보았다.  
AWS의 프리티어는 뭔가 사용하기 아까워서 다른 서비스를 찾아보던 중, 헤로쿠가 무료인데다가 빠르고 간편하다는 점이 너무 매력적이라서 사용하기로 했다.  

<center><img width="366" alt="업로드용 오류 1" src="https://user-images.githubusercontent.com/83005178/169027183-4826963e-376b-4f12-b5ae-df18e89fce1b.png">
</center>  

그러나 배포하는 과정에서 위와 같은 에러를 맞닥뜨렸고, 안내하는대로 로그를 조회해보기로했다.  

`heroku logs --tail`로 조회한 에러 코드의 일부는 다음과 같았다.  

```
Error R10 (Boot timeout) -> Web process failed to bind to $PORT within 60 seconds of launch
```  
<br>
port 번호가 잘못된 건가? 프로세스가 충돌이 나는 건가? 배포라는걸 처음 해보다 보니 정말 여러 가지 생각이 들었다.  
서칭을 해봤더니, 이런 답변을 찾을 수 있었다.  

<br><br>

<img width="488" alt="stackoverflow" src="https://user-images.githubusercontent.com/83005178/169028599-25b87904-fce3-44f9-b787-de914b403a2f.PNG">  

[StackOverflow](https://stackoverflow.com/questions/15693192/heroku-node-js-error-web-process-failed-to-bind-to-port-within-60-seconds-of)  

<br>

 요약하자면 **헤로쿠는 동적으로 앱에 포트를 할당하기 때문에, 고정된 번호로 포트를 설정할 수 없다는 이야기였다.**  
이 키 값은 `PORT`이기때문에 `process.env.PORT`형태로 바꿔 줘야했다.  


```javascript
var port = process.env.PORT || 3000;

server.listen(port, () => {
  console.log('server on! http://localhost:' + port);
});
```  
이렇게 수정해주고, `git push heroku master` 를 이용해서 다시 배포하였더니 성공적으로 끝마칠 수 있었다.