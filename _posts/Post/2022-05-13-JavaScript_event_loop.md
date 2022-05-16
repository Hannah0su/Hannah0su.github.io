---
title: "[JavaScript] 호출 스택과 이벤트 루프"
categories:
  - JS
tags:
  - JS
  - TIL

toc: true
toc_sticky: true

date: 2022-05-16
last_modified_at: 2022-05-16
---

<br>

# **호출 스택과 이벤트 루프**  
<br>

최근 ```Node.js```를 이용한 간단한 프로젝트를 진행하고 있습니다.  
프로젝트를 진행하면서, 자바스크립트에 대한 개념들을 일부 다잡을 필요를 느껴, 공부한 내용을 정리해보았습니다.  

---

## 호출 스택  
<br>

먼저 V8과 같은 자바스크립트 엔진은 단일 호출 스택(Call Stack)을 사용하며, 요청(Request)이 들어올 때마다 해당 요청을 호출 스택에 담아 순차적으로 처리합니다. (LIFO)  

이 동기적 실행 구조를 그림으로 그려보면 다음과 같습니다.    
![stack js](https://user-images.githubusercontent.com/83005178/168032826-36b94833-23be-46e3-a927-bd26193a2135.png)

자바스크립트의 동기적 실행은 위와 같은 순서로 실행됩니다.  

파일이 실행될 때 가상의 전역 컨텍스트가 맨 밑에 쌓이고, 함수들이 호출 순서대로 쌓이게 됩니다.  
파일의 실행이 끝날 때, Main()이 호출 스택에서 빠져나감으로써, 스택이 비게 되고 비로소 자바스크립트의 실행이 완료되는 것입니다.  


```javascript
function a() {
  b();
  console.log('Hannah0su!!!');
}
function b() {
  c();
  console.log('my name is');
}
function c() {
  console.log('Hii!!!');
}
a();
```  

그렇다면 위의 코드는 콘솔 창에 차례대로 `Hii!!!`,`my name is`,`Hannah0su!!!` 이 출력될 것입니다.

---
## 이벤트 루프  
<br>

그렇다면, `setTimeout` 등의 비동기 함수의 실행은 어떨까요?  
이 구조는 호출 스택만으로는 설명하기에 애로사항이 있습니다.  
이때 등장하는 개념이 `이벤트 루프`입니다.

```javascript
function run() {
  console.log('3초 후 실행');
}
console.log('시작');
setTimeout(run,3000);
console.log('끝');
```
위 코드의 실행 순서를 그림으로 그려보겠습니다.  

<br>
<br>

![diagram](https://user-images.githubusercontent.com/83005178/168576632-b7a7774d-1fce-4782-9b86-2f700d7c1149.jpg)

`function run()`이 선언되어 메모리에 저장되어 있을 것입니다.  
그리고 `console.log('시작')`이 호출 스택에 쌓입니다.  
<br>

![diagram (1)](https://user-images.githubusercontent.com/83005178/168576684-06936b7f-d4ff-46ee-9777-103d7849b0d6.jpg)  

이후 `console.log('시작')`이 실행되어 콘솔에 `시작`이 출력됩니다.  
<br>

![diagram (2)](https://user-images.githubusercontent.com/83005178/168576715-108a5252-896e-467a-986b-c3a492f2eebd.jpg)  

다음으로 `setTimeout(run,3000)`이 호출 스택에 쌓이게 됩니다.  
<br>

![diagram (3)](https://user-images.githubusercontent.com/83005178/168576742-0ef34f8d-cba5-4427-b0d3-292e1c86f9da.jpg)  

`setTimeout(run,3000)`이 실행되면서 호출 스택에서 빠져나오고, 백그라운드에 타이머와 함께 `run`함수를 싣습니다.  
코드가 백그라운드로 가게 되면 호출 스택과 백그라운드가 동시에 실행되게 됩니다.  
<br>

![diagram (4)](https://user-images.githubusercontent.com/83005178/168576766-0ca05896-c85b-4575-90a5-734285c2bfd8.jpg)  

`console.log('끝')`이 호출 스택에 실리고, 실행되는 와중에도 백그라운드의 타이머는 동시에 실행되고 있습니다.  
<br>

![diagram (6)](https://user-images.githubusercontent.com/83005178/168576818-fdb28419-20e3-4f4b-a8f1-962719ca1445.jpg)  


백그라운드의 타이머는 3초를 센 뒤 run 함수를 태스크 큐로 보냅니다.  
<br>

![diagram (7)](https://user-images.githubusercontent.com/83005178/168576852-8a00aeb1-411b-4e3b-a7a4-1c0021d2dd0f.jpg)  

호출 스택이 모두 빈 것을 확인하면 이벤트 루프가 태스크 큐에서 차례대로 함수를 호출 스택에 싣습니다.  
<br>

![diagram (8)](https://user-images.githubusercontent.com/83005178/168576879-7ede2959-ecb1-49c3-8bef-bf6aba716661.jpg)  

`run`함수 안의 `console.log('3초 후 실행)`이 호출 스택에 쌓이게 됩니다.  
<br>

![diagram (9)](https://user-images.githubusercontent.com/83005178/168576913-fb15e085-4884-4bfb-9fba-e321df43a6b6.jpg)  

`3초 후 실행`이 콘솔에 출력되고 호출 스택에서 `run`함수가 빠져나가게 되면서, 태스크 큐에 더 남은 작업이 없다면 자바스크립트의 실행이 종료되게 됩니다.  
<br>

---

## 마이크로 태스크 큐? 잡 큐?

사실 이외에도, `Promise.then`,`Promise.catch`,`process.nextTick` 등의 특별한 경우가 있습니다.  
이들은 태스크 큐가 아닌 별도의 마이크로 태스크 큐에 싣습니다.  
마이크로 태스크 큐는 태스크 큐의 작업보다 더 높은 우선순위를 갖습니다.  
태스크 큐에 대기중인 작업이 있더라도, 이벤트 루프는 태스크 큐 대신 마이크로 태스크 큐가 비었는지 먼저 확인하고, 실행합니다.  

---
#### 마치며  
<br>

이벤트 루프를 완벽하게 이해했다고 하기에는, 이 포스팅에서 다룬 것 이상으로 방대한 내용들이 아직 남아있습니다.  
여기서 만족하지말고, 문서들을 읽으면서 꼼꼼하게 공부해가야겠습니다.  


---
#### 참고 링크  
<br>

- 문서
  - [ECMAScript 6th Edition](https://www.ecma-international.org/ecma-262/6.0/index.html)  
  - [HTML Living Standard](https://html.spec.whatwg.org/)  
- 블로그 
  - [제로초님 블로그](https://www.zerocho.com/category/JavaScript/post/597f34bbb428530018e8e6e2)  
  - [NHN 기술블로그](https://meetup.toast.com/posts/89)  

- visualize
  - [해외 블로그](https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif)  
  - [latentflip](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)  

