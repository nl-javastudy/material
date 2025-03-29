# Chapter 2: 객체지향 프로그래밍 (OOP)

## 1. 클래스와 객체

### 1.1 클래스의 정의
```java
public class Student {
    // 필드 (멤버 변수)
    private String name;
    private int age;
    
    // static 필드 (클래스 변수)
    private static int totalStudents = 0;
    
    // 생성자
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        totalStudents++;
    }
    
    // 인스턴스 메서드
    public void study() {
        System.out.println(name + "이(가) 공부합니다.");
    }
    
    // static 메서드 (클래스 메서드)
    public static int getTotalStudents() {
        return totalStudents;
    }
}
```

### 1.2 클래스와 인스턴스의 차이
- **클래스**: 객체의 설계도, 타입
- **인스턴스**: 클래스로부터 생성된 객체
- **static vs non-static**
  - static: 클래스에 속하는 멤버
  - non-static: 인스턴스에 속하는 멤버

## 2. 필드와 메서드

### 2.1 필드 (멤버 변수)
- **인스턴스 변수**: 각 인스턴스마다 다른 값을 가짐
- **클래스 변수**: 모든 인스턴스가 공유하는 값
- **지역 변수**: 메서드 내에서만 사용되는 변수

### 2.2 메서드
- **인스턴스 메서드**: 인스턴스의 상태를 변경하거나 조회
- **클래스 메서드**: 인스턴스와 무관하게 동작
- **메서드 오버로딩**: 같은 이름, 다른 매개변수

## 3. 생성자

### 3.1 생성자의 특징
- 클래스와 같은 이름
- 반환 타입 없음
- 오버로딩 가능
- 기본 생성자 자동 제공

### 3.2 생성자 예제
```java
public class Car {
    private String brand;
    private String model;
    private int year;
    
    // 기본 생성자
    public Car() {
        this.brand = "Unknown";
        this.model = "Unknown";
        this.year = 2024;
    }
    
    // 매개변수가 있는 생성자
    public Car(String brand, String model, int year) {
        this.brand = brand;
        this.model = model;
        this.year = year;
    }
}
```

## 4. this 키워드

### 4.1 this의 용도
- 인스턴스 자신의 참조
- 생성자에서 다른 생성자 호출
- 매개변수와 필드 이름이 같을 때 구분

### 4.2 this 예제
```java
public class Person {
    private String name;
    
    public Person(String name) {
        this.name = name;  // 필드와 매개변수 구분
    }
    
    public Person() {
        this("Unknown");  // 다른 생성자 호출
    }
}
```

## 5. 접근 제어자

### 5.1 접근 제어자의 종류
- **private**: 같은 클래스 내에서만 접근 가능
- **default**: 같은 패키지 내에서만 접근 가능
- **protected**: 같은 패키지와 자식 클래스에서 접근 가능
- **public**: 모든 클래스에서 접근 가능

### 5.2 접근 제어자 예제
```java
public class AccessExample {
    private int privateVar;    // 같은 클래스 내에서만
    int defaultVar;           // 같은 패키지 내에서만
    protected int protectedVar; // 같은 패키지와 자식 클래스
    public int publicVar;     // 모든 클래스
}
```

## 6. 추상 클래스와 인터페이스

### 6.1 추상 클래스
- **특징**
  - 추상 메서드를 포함할 수 있음
  - 인스턴스 생성 불가
  - 일반 메서드와 필드 포함 가능

### 6.2 인터페이스
- **특징**
  - 모든 메서드가 추상 메서드 (Java 8 이후 default 메서드 추가)
  - 상수만 포함 가능
  - 다중 구현 가능

### 6.3 예제
```java
// 추상 클래스
abstract class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    abstract void makeSound();
    
    public void eat() {
        System.out.println(name + "이(가) 먹습니다.");
    }
}

// 인터페이스
interface Flyable {
    void fly();
}

// 구현
class Bird extends Animal implements Flyable {
    public Bird(String name) {
        super(name);
    }
    
    @Override
    void makeSound() {
        System.out.println("짹짹");
    }
    
    @Override
    public void fly() {
        System.out.println(name + "이(가) 날아갑니다.");
    }
}
```

## 7. 패키지와 import

### 7.1 패키지
- 클래스들의 그룹화
- 이름 충돌 방지
- 접근 제어와 관련

### 7.2 import
- 다른 패키지의 클래스 사용
- static import
- 와일드카드 import

## 8. java.lang 패키지

### 8.1 Object 클래스
- 모든 클래스의 조상
- 주요 메서드
  - toString(): 객체의 문자열 표현 반환
  - equals(): 객체의 동등성 비교
  - hashCode(): 객체의 해시 코드 반환
  - getClass(): 객체의 클래스 정보 반환
  - clone(): 객체의 복사본 생성

### 8.2 래퍼 클래스
- 기본형을 객체로 감싸는 클래스
- **종류**
  - Byte, Short, Integer, Long
  - Float, Double
  - Character
  - Boolean

### 8.3 오토박싱과 언박싱
```java
// 명시적 박싱
Integer num1 = Integer.valueOf(10);  // int -> Integer
Double d1 = Double.valueOf(3.14);    // double -> Double

// 명시적 언박싱
int n1 = num1.intValue();            // Integer -> int
double d2 = d1.doubleValue();        // Double -> double

// 오토박싱
Integer num2 = 10;  // int -> Integer

// 언박싱
int n2 = num2;      // Integer -> int
```

## 9. 실습 예제 코드

### 9.1 Object 클래스 메서드 예제
```java
// Person.java
public class Person implements Cloneable {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public String toString() {
        return "Person[name=" + name + ", age=" + age + "]";
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return age == person.age && name.equals(person.name);
    }
    
    @Override
    public int hashCode() {
        return 31 * name.hashCode() + age;
    }
    
    @Override
    public Person clone() throws CloneNotSupportedException {
        return (Person) super.clone();
    }
}

// ObjectTest.java
public class ObjectTest {
    public static void main(String[] args) throws CloneNotSupportedException {
        Person p1 = new Person("홍길동", 20);
        Person p2 = new Person("홍길동", 20);
        Person p3 = p1.clone();
        
        // toString() 테스트
        System.out.println("p1: " + p1.toString());
        
        // equals() 테스트
        System.out.println("p1.equals(p2): " + p1.equals(p2)); // 주소가 아니라 값 자체를 비교!!
        
        // hashCode() 테스트
        System.out.println("p1.hashCode(): " + p1.hashCode());
        System.out.println("p2.hashCode(): " + p2.hashCode());
        
        // getClass() 테스트
        System.out.println("p1.getClass(): " + p1.getClass());
        
        // clone() 테스트
        System.out.println("p3: " + p3.toString());
        System.out.println("p1 == p3: " + (p1 == p3)); // 주소가 같을까? 다를까?
    }
}
```

### 9.2 래퍼 클래스 예제
```java
// WrapperTest.java
public class WrapperTest {
    public static void main(String[] args) {
        // 명시적 박싱
        Integer num1 = Integer.valueOf(100);
        Double d1 = Double.valueOf(3.14);
        Boolean b1 = Boolean.valueOf(true);
        
        // 명시적 언박싱
        int n1 = num1.intValue();
        double d2 = d1.doubleValue();
        boolean b2 = b1.booleanValue();
        
        // 오토박싱
        Integer num2 = 200;
        Double d3 = 2.718;
        Boolean b3 = false;
        
        // 언박싱
        int n2 = num2;
        double d4 = d3;
        boolean b4 = b3;
        
        // 출력
        System.out.println("명시적 박싱 결과:");
        System.out.println("num1: " + num1);
        System.out.println("d1: " + d1);
        System.out.println("b1: " + b1);
        
        System.out.println("\n명시적 언박싱 결과:");
        System.out.println("n1: " + n1);
        System.out.println("d2: " + d2);
        System.out.println("b2: " + b2);
        
        System.out.println("\n오토박싱 결과:");
        System.out.println("num2: " + num2);
        System.out.println("d3: " + d3);
        System.out.println("b3: " + b3);
        
        System.out.println("\n언박싱 결과:");
        System.out.println("n2: " + n2);
        System.out.println("d4: " + d4);
        System.out.println("b4: " + b4);
    }
}
```


### 10. 실습
1. 9번의 예제 코드 2개를 직접 따라 쳐보세요. (각각의 자바 파일로 만드세요) 그리고 실행해보세요.

2. 피타고라스의 정리 문제(https://school.programmers.co.kr/learn/courses/30/lessons/250132)를 자바로 풀어보세요.  
> 스캐너 설명
> ```java
> // Scanner 사용법
> import java.util.Scanner;
> 
> public class ScannerExample {
>     public static void main(String[] args) {
>         // Scanner 객체 생성
>         Scanner sc = new Scanner(System.in);
>         
>         // 정수 입력 받기
>         int num = sc.nextInt();
>         
>         // 실수 입력 받기
>         double d = sc.nextDouble();
>         
>         // 문자열 입력 받기
>         String str = sc.next();     // 공백 전까지의 문자열
>         String line = sc.nextLine(); // 한 줄 전체 문자열
>         
>         // 입력 버퍼 비우기
>         sc.nextLine();  // next() 후 nextLine()을 사용할 때 필요
>         
>         // Scanner 닫기 (OS에서 제공하는 stdin을 사용 다했으면 돌려줘야함. GC와는 관계X)
>         sc.close();
>     }
> }
> ```
> 
> **Scanner 주요 메서드**
> - `nextInt()`: 정수 입력
> - `nextDouble()`: 실수 입력
> - `next()`: 공백 전까지의 문자열 입력
> - `nextLine()`: 한 줄 전체 문자열 입력
> - `hasNext()`: 다음 입력이 있는지 확인
> - `close()`: Scanner 닫기
> 
> **주의사항**
> 1. `next()`와 `nextLine()`의 차이
>    - `next()`: 공백 전까지의 문자열만 입력받음
>    - `nextLine()`: 엔터키까지의 모든 문자열 입력받음
> 
> 2. `next()` 후 `nextLine()` 사용 시 주의
>    - `next()`가 남긴 엔터키를 `nextLine()`이 읽어버림
>    - 해결: `nextLine()`을 한 번 더 호출하여 엔터키 제거
> 
> 3. Scanner 닫기
>    - 프로그램 종료 전에 `close()` 호출 권장
>    - System.in을 사용하는 다른 Scanner에도 영향



3. 아이스 아메리카노 문제(https://school.programmers.co.kr/learn/courses/30/lessons/120819)를 자바로 풀어보세요. 아이스 아메리카노의 가격을 final static을 붙여 필드로 선언해보세요.

4. 문자 반복 출력하기 문제(https://school.programmers.co.kr/learn/courses/30/lessons/120825)를 자바로 풀어보세요. 아래의 항목중에 마음에 드는 것을 골라서 사용해보세요.
> StringBuilder  
> string.split()  
> string.toCharArray()  
> string.repeat()  

5. 3번과 4번 문제 풀이를 .java나 .txt 파일로 붙여넣어서 본인의 학습노트에 올리고 커밋 후 원격 리포지토리에 푸시하세요.

### 11. 세션 배부
- Exception(예외) vs Error(에러) 차이점
- Checked Exception(체크 예외) vs Unchecked Exception (언체크 예외)
- RuntimeException 에 대하여
- NullPointerException 에 대하여
- ArrayIndexOutOfBoundsException 에 대하여
- throws와 throw (예외 던지기)
- try, catch, finally (예외 잡기)