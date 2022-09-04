---
title: "Java - I/O스트림"
date: 2022-09-04
tags:
  - java
series: "java-programming"
---

## 입출력 스트림(I/O 스트림)

* 자바는 다양한 입출력 장치에 독립적으로 일관성있는 입출력을 입출력 스트림을 통해 제공한다.
* 입출력이 구현되는 곳: 파일 디스크, 키보드, 마우스, 네트웍, 메모리 등 모든 자료가 입력되고 출력되는 곳



## 입출력 스트림의 구분

* 대상 기준 : 입력 스트림 / 출력 스트림
* 자료의 종류 : 바이트 스트림 / 문자 스트림
* 기능 : 기반 스트림 / 보조 스트림



### 📌 기반 스트림과 보조 스트림

* 기반 스트림 : 대상에 직접 자료를 읽고 쓰는 기능의 스트림
* 보조 스트림 : 직접 읽고 쓰는 기능은 없이 추가적인 기능을 더해주는 스트림
* 보조 스트림은 직접 읽고 쓰는 기능은 없으므로 항상 기반 스트림이나 또 다른 보조 스트림을 생성자의 매개 변수로 포함함

![](io.png)

<br/>

| 종류        | 예시                                                         |
| ----------- | ------------------------------------------------------------ |
| 기반 스트림 | FileInputStream, FileOutputStream, FileReader, FileWriter 등 |
| 보조 스트림 | InputStreamReader, OutputStreamWriter, BufferedInputStream, BufferedOutputStream 등 |



## System 클래스의 표준 입출력 멤버

```java
public class System{ 
	public static PrintStream out; 
	public static InputStream in; 
	public static PrintStream err; 
}
```

> * System.out : 그동안 자주 사용했던 표준 출력(모니터) 스트림이다. 
>   * System.out.println("hello");
> * System.in : 표준 입력(키보드) 스트림이다.
>   * int i = System.in.read();
> * System.err : 표준 에러 출력(모니터) 스트림
>   * System.err.println("에러 메세지");



### 📌 예시 - System.in

```java
// 문자열 하나 받기
int i;
try{
    i = System.in.read(); // read()의 반환값은 int형이다.
    System.out.println((char)i);
} catch (IOException e){
    e.printStackTrace();
}

// 문자열 여러 개 받기
int i;
try{
    while((i = System.in.read()) != '\n'){// 엔터를 치면 종료
    System.out.print((char)i);
    }
} catch (IOException e){
    e.printStackTrace();
}
```





## 바이트 단위 스트림

### 📌 InputStream

> 바이트 단위 입력 스트림의 최상위 추상 클래스

* 많은 추상 메서드가 선언되어 있고 이를 하위 스트림이 상속받아 구현한다.



### 📌 InputSteam의 주요 하위 클래스

| 스트림 클래스        | 설명                                                         |
| -------------------- | ------------------------------------------------------------ |
| FileInputStream      | 파일에서 바이트 단위로 자료를 읽습니다.                      |
| ByteArrayInputStream | byte 배열 메모리에서 바이트 단위로 자료를 읽습니다.          |
| FilterInputStream    | 기반 스트림에서 자료를 읽을 때 추가 기능을 제공하는 보조 스트림의 상위 클래스 |



### 📌 InputSteam의 주요 메서드

| 메서드                               | 설명                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| int read()                           | 입력 스트림으로부터 한 바이트의 자료를 읽습니다. 읽은 자료의 바이트 수를 반환합니다. |
| int read(byte b[])                   | 입력 스트림으로 부터 b[] 크기의 자료를 b[]에 읽습니다. 읽은 자료의 바이트 수를 반환합니다. |
| int read(byte b[], int off, int len) | 입력 스트림으로 부터 b[] 크기의 자료를 b[]의 off변수 위치부터 저장하며 len 만큼 읽습니다. 읽은 자료의 바이트 수를 반환합니다. |
| void close()                         | 입력 스트림과 연결된 대상 리소스를 닫습니다.                 |

> 리소스를 읽고 나선 반드시 close()를 해줘야 함! 



### 📌 예시 - FileInputStream

```java
// input.txt
abc
```

```java
FileInputStream fis = null; 
		// try문안에서 인풋스트림을 정의할 경우 finally문에서 사용하지 못함
		// 미리 null로 선언해준다
		try {
			fis = new FileInputStream("input.txt");
			int i;
			while ((i = fis.read()) != -1) // 더 읽어올 바이트가 없다면 -1을 리턴
			System.out.print((char)i);

		}catch(Exception e) {
			e.printStackTrace();
		}finally {
			try {
				fis.close();
			} catch (Exception e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
```

> 출력결과 : abc
