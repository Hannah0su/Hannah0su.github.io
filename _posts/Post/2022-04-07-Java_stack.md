---
title: "[Java] 스택(STACK)"
categories:
  - Java
tags:
  - Java
  - TIL

toc: true
toc_sticky: true

date: 2022-04-07
last_modified_at: 2022-04-07
---
<br>

# **STACK**  
<br>

알고리즘과 자료구조 공부를 시작하면서 알게 된 Stack에 관해 정리하여 포스팅해보려고 합니다.  <br>
[문서](https://docs.oracle.com/javase/10/docs/api/java/util/Stack.html)와 [visualgo](https://visualgo.net/en/list)가 큰 도움이 되었습니다.  <br>
특히 **visualgo**의 시각화자료가 굉장히 잘돼있어서 감동했습니다.  <br>
앞으로도 공부할 때 자주 신세 져야겠습니다.😁



---

## 스택(Stack)이란?
<br>

스택(Stack)의 사전적 의미로는 살펴보면 무더기, 더미,~를 쌓다 등이 있습니다.

쉽게 생각해보면, 창고에 박스를 쌓는 행위를 떠올리시면 됩니다. 박스를 밑에서부터 차곡차곡 쌓기 시작한다면, 결국 가장 먼저 쌓은 박스는 맨 밑부분에, 맨 마지막에 쌓은 박스는 최상단에 위치하게 됩니다.

이 박스를 다른 곳에 옮기거나 사용하려면, 자연스럽게 가장 위에 쌓은 박스부터 꺼내고, 마지막에서야 맨 밑에 깔린 박스를 꺼내게 됩니다.

스택 구조도 이처럼 데이터를 쌓는 형식으로 저장하는데, 마지막에 들어온 데이터가 먼저 나가게 되는, **후입선출(LIFO,Last In First Out)**의 구조를 띠게 됩니다.

인터넷 브라우저의 뒤로 가기 버튼을 누르면 가장 최근에 방문했던 사이트가 나오게 됩니다.
모두들 자주 사용하는 Undo(ctrl+z) 기능은 가장 최근에 실행한 작업을 취소합니다. 이 기능들 모두 스택의 구조를 활용한 기능입니다.

<br>
<br> 

---

## Java에서의 Stack Class
<br>

기본적으로 자바에서 지원해주는 스택 클래스의 메소드들에 대해서 알아보겠습니다.

```java
Stack<Integer> stack = new Stack<>(); //int형
Stack<String> stack = new Stack<>(); //char형
```
먼저 선언은 위와 같이 합니다.  <br>

---

```java
stack.push();//Stack에 요소를 추가하고 반환
```
```.push(E item)``` : 스택의 최상단에 요소를 추가합니다.  <br>

---


```java
stack.pop(); //Stack 최상단의 요소 제거하고 반환
stack.clear();//Stack의 모든 요소를 제거
```

```.pop()``` : 스택의 최상단의 요소를 제거하고, 그 값을 반환합니다.  <br>
```.clear()``` : 스택 내의 모든 요소를 제거합니다. 이에 따라 스택의 size도 0으로 초기화됩니다.  <br>

---

```java
stack.peek();//Stack의 최상단의 값 반환
```
```.peek()``` : 스택의 최상단에 있는 요소를 제거하지 않고, 반환합니다.  <br>
```pop```과 달리 데이터를 제거하지 않고 확인할 수 있습니다.  <br>

---

```java
stack.search()//지정된 객체의 위치를 반환,없을경우 -1을 반환
```
```.search​(Object o)``` : 지정한 객체의 위치를 반환합니다.  <br>
이때 조심할 것은 search는 찾고자 하는 객체가 스택의 Top으로부터 몇 번째 위치에 존재하는지를 반환하는 것입니다.  

```java
stack.push(1);
stack.push(2);
stack.push(3);
System.out.println(stack.search(3)); //1 반환
System.out.println(stack.search(2)); //2 반환
System.out.println(stack.search(5));//-1 반환
```
지정된 객체가 스택에 존재하지 않으면 -1을 반환합니다.  <br>

---

```java
stack.size();//Stack의 크기 반환
stack.empty();//Stack이 비어있다면 True,아니라면 False를 반환
```
```.size()``` : 스택의 크기를 반환합니다. 스택의 크기는 요소의 개수와 동일합니다.  <br>
```.empty()``` : 스택에 요소가 존재하는지 여부를 검사하여 Boolean 값으로 반환합니다.   <br>

---
## 배열을 이용한 stack 구현

간단한 문제를 하나 풀어보겠습니다.

[백준 10828번](https://www.acmicpc.net/problem/10828)

구현해야 할 기능은 push,pop,size,empty,top으로 5가지 입니다.  <br>
다른 명령어는 실행 후 출력을 요구하지만, push는 출력을 요구하지 않습니다.  <br>
배열의 인덱스가 0부터 시작한다는 점과 stack의 구조를 염두에 두면서 구현하면 간단하게 풀 수 있겠습니다.  <br>

```java
import java.io.OutputStreamWriter;
import java.io.InputStreamReader;
import java.io.IOException;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.util.StringTokenizer;

public class Main {
 
	public static void main(String[] args) throws IOException {
		
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		
		
		int n = Integer.parseInt(br.readLine());
		
		int [] stack = new int[n];  //요소를 담을 stack배열 선언
		int size = 0; //스택의 사이즈를 0으로 초기화
		int cnt=0;
		
		StringTokenizer input;
		
		while(cnt<n) {
			
			input = new StringTokenizer(br.readLine()," "); //공백을 기준으로 문자열 잘라내기
			
		
			switch (input.nextToken()) {
			
			case "push":
				stack[size] = Integer.parseInt(input.nextToken());//stack에 요소를 추가
				size++;     //size 증가
				break;
				
			case "pop":
				if (size==0) {
					bw.write(-1+"\n");  //stack에 요소가 0개면 -1을 반환
					
				}
				else {
					bw.write(stack[size-1]+"\n");//배열의 인덱스는 0부터 시작하기때문에 최상단의 값은 size-1
					stack[size-1] = 0;  //최상단의 값을 0으로 초기화
					size--;             //size 감소
				}
				break;
				
			case "size":
				bw.write(size+"\n");//stack의 요소 개수를 반환
				break;
				
			case "empty":
				if(size == 0) {
					bw.write(1+"\n");//stack이 비어있으면 1을 반환(True)
				}
				else {
					bw.write(0+"\n");//stack이 비어있지않다면 0을 반환(False)
				}
				break;
				
			case "top":
				if(size == 0) {
					bw.write(-1+"\n");//stack이 empty상태라면 -1을 반환
				}
				else {
					bw.write(stack[size-1]+"\n");//stack의 최상단 값을 반환
				}
				break;
				
				}
			cnt++;//명령 1회 실행 후 증가
			
			
		}
		br.close();
		bw.flush();
		bw.close();
		}
	}
```

<br>

<img width="611" alt="ddd" src="https://user-images.githubusercontent.com/83005178/162128840-1fcc05ca-64cb-4b17-b614-ea67e0856a82.PNG">

