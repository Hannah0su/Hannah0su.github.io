---
title: "[Linux] 리눅스 기본 명령어"
categories:
  - Linux
tags:
  - Linux
  - TIL

toc: true
toc_sticky: true

date: 2022-03-31
last_modified_at: 2022-03-31
---
<br>

# **리눅스 기본 명령어**  
<br>
명령어의 뒤에 ```--help``` 를 삽입하여 상세 설명을 볼 수 있습니다.  
또는 ```man``` 명령어를 사용하여  간단한 설명을 볼 수 있습니다.  

---

## 🔍 명령어 요약  

|**명령어**|**기능**|
|:---------:|:---------------:|
|[ls](#ls)|List Segments의 약자, 윈도우의 dir 명령어에 해당하는 기능|
|[cd](#cd)|Change Directory의 약자, 디렉터리를 이동하는 명령어|
|[pwd](#pwd)|Print Working Directory의 약자, 현재 디렉터리의 전체 경로를 화면에 출력|
|[touch](#touch)|크기가 0인 새 파일을 생성하거나, 이미 파일이 존재한다면 파일의 최종 수정 시간을 변경|
|[mkdir](#mkdir)|Make Directory의 약자, 새로운 디렉터리를 생성|
|[rmdir](#rmdir)|Remove Directory의 약자, 디렉터리를 삭제|
|[cp](#cp)|Copy의 약자, 파일이나 디렉터리를 복사|
|[rm](#rm)|Remove의 약자, 파일이나 디렉터리를 삭제|
|[mv](#mv)|Move의 약자, 파일이나 디렉터리의 이름을 변경하거나 다른 디렉터리로 이동할 때 사용|
|[cat](#cat)|Concatenate의 약자, 파일의 내용을 화면에 출력|
|[head, tail](#headtail)|텍스트 형식으로 작성된 파일의 앞 10행 또는 마지막 10행만 화면에 출력|
|[more](#more)|텍스트 형식으로 작성된 파일을 페이지 단위로 화면에 출력|
|[less](#less)|more 명령어와 용도가 비슷하지만 더 확장된 기능을 갖춘 명령어|
|[file](#file)|해당 파일이 어떤 종류의 파일인지 보여줌|
|[clear](#clear)|현재 사용중인 터미널 화면을 깨끗이 지워줌|


---

### ls  

```
#ls	-- 현재 디렉터리의 파일 목록을 보여줌

#ls /etc/systemd -- 디렉터리의 목록을 보여줌

#ls -a --현재 디렉터리의 목록(숨김 파일 포함)을 보여줌

#ls -l -- 현재 디렉터리의 목록을 자세히 보여줌

#ls *.conf --확장자가 .conf인 목록을 보여줌(예시에서는 conf)

#ls -l /etc/systemd/b* -- etc/systemd 디렉터리에 있는 목록 중 앞 글자가 b인 것을 자세히 보여줌

```
---

### cd  

```
#cd -- 현재 사용자의 홈 디렉터리로 이동. ex)만약 현재 사용자가 root라면 /root 디렉터리로 이동

#cd ~ubuntu --ubuntu 사용자의 홈 디렉터리로 이동

#cd .. -- 바로 상위의 디렉터리로 이동 '..'는 현재 디렉터리의 부모 디렉터리를 의미한다 (ex: 현재 디렉터리가 etc/systemd라면 /etc 디렉터리로 이동)

#cd /etc/sytemd -- /etc/systemd 디렉터리로 이동 (절대 경로)

#cd ../etc/systemd -- 상대 경로로 이동. 현재 디렉터리의 상위(..)로 이동한 후 /etc/systmed로 이동
```

---

### pwd  

```
#pwd -- 현재 작업 중인 디렉터리의 경로 출력
```
---

### touch  

```
#touch test.txt -- 파일이 없으면 test.txt라는 빈 파일을 생성하고, test.txt가 있으면 파일의 최종 수정 시간을 현재 시간으로 변경
```
---

### mkdir  

```
#mkdir abc -- 현재 디렉터리 아래에 /abc 라는 디렉터리를 생성
#mkdir -p /def/fgh --/def/fgh 디렉터리 생성. 만약 /fgh의 부모 디렉터리인 /def 디렉터리가 없으면 자동 생성 (p: parents,부모)
```
---

### rmdir  

```
#rmdir abc -- /abc 디렉터리 삭제
```
---

### cp  

```
#cp abc.txt cba.txt -- abc.txt의 파일명을 cba.txt로 바꾸어 복사
#cp -r abc cba -- 디렉터리 복사. abc 디렉터리를 cba 디렉터리로 바꾸어 복사
```
---

### rm  

```
#rm abc.txt -- 해당 파일 삭제 (rm -f)
#rm -i abc.txt -- 삭제 시 정말 삭제할지 확인하는 메시지 출력
#rm -f abc.txt -- 삭제 시 확인하지 않고 바로 삭제(f:force)
#rm -r abc -- abc 디렉터리와 그 하위 디렉터리를 강제로 모두 삭제. 편리하지만 주의해서 사용 해야 함(r:recursive)
```
---

### mv  

```
#mv abc.txt /etc/systemd/ -- abc.txt를 /etc/systemd/ 디렉터리로 이동
#mv aaa bbb ccc dd -- aaa,bbb,ccc 파일을 /ddd 디렉터리로 이동
#mv abc.txt www.txt -- abc.txt의 파일명을 www.txt로 변경
```
---

### cat  

```
#cat a.txt b.txt -- a.txt와 b.txt를 연결하여 파일의 내용을 화면에 출력
```
---

### head,tail  

```
#head /etc/systemd/bootchart.conf -- 해당 파일의 앞 10행을 화면에 출력
#head -5 /etc/systemd/bootchart.conf -- 해당 파일의 앞 5행만 화면에 출력
#head -5 /etc/systemd/bootchart.conf -- 해당 파일의 마지막 5행만 화면에 출력
```
---

### more  

텍스트 형식으로 작성된 파일을 페이지 단위로 화면에 출력  
space bar를 누르면 다음 페이지로 이동,  
B를 누르면 앞 페이지로 이동,  
Q를 누르면 종료  

```
#more /etc/systmed/system.conf
#more +10 /etc/systemd/system.conf -- 해당 파일의 10행부터 출력
```
---

### less  

more 명령어와 용도가 비슷하지만 더 확장된 기능의 명령어  
more 명령어에서 사용하는 키도 사용할 수 있음  
**추가로 ↑,↓,←,→, PageUp,PageDown 사용 가능**  

```
#less /etc/systemd/system.conf
#less +10 /etc/systemd/system.conf -- 해당 파일의 10행부터 출력
```
---

### file  

```
#file /etc/systemd/system.conf -- system.conf는 텍스트 파일이므로 아스키 파일(ASCII)로 표시
#file /bin/gzip -- gzip은 실행 파일이므로 ELF 64-bit LSB executable 파일로 표시
```
---

### clear  

```
#clear -- 현재 사용 중인 터미널 화면을 깨끗하게 지워줌
```