---
title: "[Java] 자바에서의 빠른 입출력"
categories:
  - Java
tags:
  - Java
  - TIL
  - Algorithm

toc: true
toc_sticky: true

date: 2022-04-01
last_modified_at: 2022-04-01
---
<br>

# **Java에서의 빠른 입출력**  
<br>

제가 Java를 처음 배운 입력 방식은 ```Scanner```였습니다.  
Scanner는 Space와 개행문자를 경계로 입력값을 인식해주기 때문에, 별다른 가공의 과정도 필요하지 않아 편리하게 사용했습니다.  

```java
int a = scanner.nextInt();
Double b = scanner.nextDouble();
```

```BufferedReader``` 는 다릅니다. 개행문자만 경계로 인식하기 때문에 입력값이 String으로 고정됩니다.  그래서 가공하는 과정을 필요로 합니다.  
<br>
![버퍼](https://user-images.githubusercontent.com/83005178/161001585-7a9af126-bb55-4d42-be0e-b7fa9b4feba0.png)
<br>
그럼에도 BufferedReader를 사용하는 이유는 빠르기 때문입니다.  
하나의 입력을 받을 때마다 I/O가 발생하면 성능에 좋지 못한 영향을 끼칩니다.  
하지만 중간에 버퍼를 둬서 메모리를 받은 다음 한번에 전송하면 훨씬 효율적이겠죠?  
BufferedReader는 [버퍼](https://ko.wikipedia.org/wiki/%EB%B2%84%ED%8D%BC_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))를 사용하여 입력 받은 값을 임시로 저장하고, 통째로 전달합니다. 
그래서 입력값이 많으면 많을수록 BufferedReader가 Scanner보다 더 좋은 성능을 보입니다.


---

## BufferedReader 사용법
<br>

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String str = bf.readLine()
int num = Integer.parseInt(br.readLine()); //형변환
```  

선언은 위와 같이 합니다. ```return```값이 ```String```이기 때문에 다른 타입은 형 변환이 필요합니다.  
또한 반드시 ```IOException```등을 이용한 **예외 처리**가 필요합니다.

<br>
<br>

```java
StringTokenizer str;
str = new StringTokenizer(br.readLine()," ");
int numA = Integer.parseInt(str.nextToken());
```
위와 같이 ```StringTokenizer```를 사용하여 입력받은 ```readLine()```의 값을 공백 단위로 끊어 호출할 수도 있습니다.
<br>

```java
String array[] = str.split(" ");
```
이렇게 ```String.split```을 이용하여 공백 단위로 끊어서 배열에 저장하여 사용하는 방법도 있습니다.


```java
br.close();
```
입력을 다 받고 나면 ```.close()```로 입력 스트림을 닫아줍니다.  

---

## BufferedWriter 사용법
<br>

출력도 마찬가지입니다. 입력값이 많으면 많을수록  ```System.out.println()```보다는 ```BufferedWriter```처럼 버퍼를 사용하는 출력의 성능이 우수합니다.


```java
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
bw.write("출력");
bw.newLine();
bw.write("출력과 함께 \n");
bw.flush();
bw.close();
```

```.newLine();```로 개행문자를 출력할 수 있습니다.  
혹은 ```\n```을 출력문 내에 삽입하여 출력할 수도 있습니다.  
출력이 끝나고 나면 ```.flush()```을 이용해서 버퍼를 비워줘야 합니다.  
```.close```로 스트림을 닫아줍니다.  

---

## 정리
<br>

간단한 문제를 하나 풀어보겠습니다.  

[백준 15552번](https://www.acmicpc.net/problem/15552)  
<br>

![screencapture-acmicpc-net-problem-15552-2022-03-31-16_59_43](https://user-images.githubusercontent.com/83005178/161016031-185f706b-de64-43a9-ab69-2e5a6bcd7b84.png)

<br>

bufferedReader와 BufferedWriter의 사용을 요구하는 문제입니다.
첫 줄에 테스트케이스의 개수를 입력하고
매 출력마다 한 줄씩 띄워 줘야합니다.

풀이는 다음과 같습니다.  

<br>

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;
 
public class Main {
 
	public static void main(String[] args) throws IOException {
 
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in)); // BufferedReader 선언
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out)); //BufferedWriter 선언

		int numA,numB = 0; // 숫자 A와 B를 저장합니다.
 
		int num = Integer.parseInt(br.readLine()); //형변환
        
		StringTokenizer str; //공백단위로 잘라내기위함
 
		for (int i = 1; i <= num; i++) {
			str = new StringTokenizer(br.readLine()," ");
			numA = Integer.parseInt(str.nextToken());//첫번째 숫자 형변환
			numB = Integer.parseInt(str.nextToken());//두번째 숫자 형변환
			bw.write(numA + numB + "\n");//개행문자와함께 연산값 출력
			}

		br.close(); //br을 닫아줍니다.
        
		bw.flush();
		bw.close(); //bw를 비우고 닫아줍니다.
 
	}
}
 
```

알고리즘 문제를 풀다가, 시간 제한이 걸려 틀리는 문제가 많아서 한번 정리해봤습니다.  
익숙하게 사용하던 Scanner가 아니라서 처음에는 헤맸지만, 알고나니 크게 어렵지는 않았습니다.  
```.lines()``` , ```.ready()``` ,```.read()``` 등 포스트에서 이야기하지않은 메소드들도 많습니다.  
하지만 [문서](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/BufferedReader.html)에 잘 정리되어있으니 읽고 사용하는데 큰 어려움은 없을 것 같습니다.

---