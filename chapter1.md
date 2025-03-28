# Chapter 1: Java 기초와 개발 환경

## 1. Java의 특징

### 1.1 Java의 주요 특징
- **객체지향적**: 모든 것이 객체로 구성
- **플랫폼 독립성**: "Write Once, Run Anywhere"
- **자동 메모리 관리**: 가비지 컬렉션
- **강력한 타입 체크**: 컴파일 시점의 타입 검사
- **멀티스레딩 지원**: 동시성 프로그래밍 용이

### 1.2 JVM (Java Virtual Machine)
- **Virtual Machine이란 무엇인가**
  - 하나의 물리적인 하드웨어 위에서 격리되어 구동되는 가상 환경
  - Hypervisor 위에서 각각 CPU, 메모리, 저장소, 네트워크 인터페이스를 갖춤

- **JVM은...**
  - OS로부터 메모리 공간을 할당받아 메모리를 자체적으로 관리
  - 독립된 런타임 환경에서 Java 클래스 파일 실행
  - 메모리에 대해 알 필요도, 건들 필요도 없다!! (C와 대비되는 부분)

- **JVM이 Java App을 실행하는 흐름**
  1. .java파일을 컴파일 한 .class파일을 Class Loader가 Runtime Data Area에 로드 (Lazy Loading)
  2. 적재된 바이트코드를 Execution Engine이 실행
  3. 실행 도중 Heap 영역에서 더 이상 참조되지 않는 object들을 GC가 제거

- **JVM의 역할**
  - 바이트코드 실행
  - 메모리 관리
  - 가비지 컬렉션
  - 스레드 관리

- **JVM 구조**
  - **Class Loader**: 클래스 파일을 로드하고 링크
  - **Runtime Data Area**
    - Method Area: 실행 시점에 사용되는 Constant Pool, field, method, constructors와 같은 데이터들이 위치한다.
    - Heap Area: 객체와 인스턴스 변수, 배열 등과 같은 값들이 위치한다.
    - Stack Area: 메서드 실행에 필요한 프레임 저장. 쓰레드가 생성될때마다 각 쓰레드에 하나씩 할당된다.
    - PC Register: 현재 실행 중인 명령어 주소 값이 저장된다. 마찬가지로 각 쓰레드에 하나씩 할당된다.
    - Native Method Stack: Java가 아닌 C, C++과 같은 언어로 작성 된 메서드 실행을 지원하는 영역. 마찬가지로 각 쓰레드에 하나씩 할당된다. 
  - **Execution Engine**: 바이트코드를 실행 (바이트코드!=기계어)
    - Interpreter: 바이트코드를 한 줄씩 읽고 실행한다. 
    - JIT Compiler: 자주 사용되는 코드, 반복되는 코드를 네이티브 머신 코드로 컴파일
  - **Garbage Collector**: 사용하지 않는 메모리 정리 (Mark -> Sweep)

* 참고: https://steadiness.dev/jvm-basics/

## 2. Java 개발 환경

### 2.1 JDK (Java Development Kit)
- **JDK 구성요소**
  - javac: 컴파일러 (.java(코드) -> .class(바이트코드))
  - java: 실행기 (.class파일 실행)
  - javadoc: 자바 코드에 적힌 주석을 이용해 HTML 문서(개발 문서)를 자동으로 생성하는 도구
  - jdb: 디버거
  - jar: 자바 클래스 파일(.class)들을 하나의 JAR 파일(.jar)로 묶어주는 압축도구

### 2.2 IDE 선택
- IntelliJ IDEA (추천)
- Eclipse
- VS Code

## 3. Java의 데이터 타입

### 3.1 기본형 (Primitive Types)
- **정수형**
  - byte (1바이트): -128 ~ 127
  - short (2바이트): -32,768 ~ 32,767
  - int (4바이트): -2,147,483,648 ~ 2,147,483,647
  - long (8바이트): -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807

- **실수형**
  - float (4바이트): ±3.4E-38 ~ ±3.4E+38
  - double (8바이트): ±1.8E-308 ~ ±1.8E+308

- **문자형**
  - char (2바이트): 유니코드 문자 (0 ~ 65,535)

- **논리형**
  - boolean (1바이트): true 또는 false

### 3.2 참조형 (Reference Types)
- **String 클래스**
  - 불변성 (Immutability)
    - 불변객체는 재할당은 가능하지만, 한번 할당하면 내부 데이터를 변경할 수 없는 객체
  - String Pool (리터럴 풀)
  - new String() vs String literal

- **배열**
  - 객체로 취급
  - length 필드
  - 다차원 배열

## 4. 변수와 상수

### 4.1 변수 선언
```java
// 타입 변수명 = 초기값;
int number = 10;
String text = "Hello";
```

### 4.2 상수 선언
```java
// final 키워드 사용
// 이 값은 상수가 되어, 변경이 불가능함!!
final int MAX_VALUE = 100;
```

### 4.3 변수 스코프
- 클래스 변수 (static)
- 인스턴스 변수
- 지역 변수

## 5. 객체 참조 예제

### 5.1 String Pool 예제
```java
String str1 = "Hello";  // String Pool에 저장
String str2 = "Hello";  // 같은 객체 참조
String str3 = new String("Hello");  // 새로운 객체 생성

System.out.println(str1 == str2);  // true
System.out.println(str1 == str3);  // false
```

### 5.2 기본형과 참조형 차이
```java
// 기본형
int a = 10;
int b = a;
b = 20;  // a는 영향 없음

// 참조형
int[] arr1 = {1, 2, 3};
int[] arr2 = arr1;
arr2[0] = 10;  // arr1도 영향 받음
```

## 6. 실습
1. 자바를 환경변수로 등록하고, 간단한 자바 프로그램(1부터 100까지 출력)을 작성한 다음 javac로 컴파일하고 바이트코드를 확인한 후 java로 실행하세요.

2. 동일한 코드를 intellij에서 실행하세요. 다른 버전의 jdk를 다운로드받은다음 그 버전으로 전환해 보세요.

3. 프로그래머스의 평균 구하기 문제(https://school.programmers.co.kr/learn/courses/30/lessons/12944)를 해결하세요.


## 7. 세션 배부
  - 클래스(new)와 인스턴스(static)
  - 메서드와 필드
  - 생성자
  - 접근 제어자
  - 추상 클래스와 인터페이스
  - 패키지와 import
  - this 키워드