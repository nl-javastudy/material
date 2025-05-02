# Chapter 3: 예외 처리 (Exception Handling)

## 1. Exception과 Error의 차이 (동건)

### 1.1 자바의 예외 처리 시스템
Java의 예외 처리 시스템은 C언어와 비교했을 때 매우 체계적임.

**C언어의 에러 처리:**
- 함수의 반환값으로 에러를 처리 (예: -1, NULL 등)
- 개발자가 직접 에러 처리 로직을 구현해야 함
- 에러 처리를 놓치기 쉬움

**Java의 예외 처리:**
- 예외 처리 전용 구문(try-catch) 제공
- 예외 객체를 통한 상세한 에러 정보 전달
- 예외 처리를 강제하는 메커니즘(Checked Exception)
- 예외의 계층 구조를 통한 체계적인 관리

> C++에도 Java와 유사한 객체지향적 컨셉이 들어간 예외처리 시스템 제공.

### 1.2 Exception vs Error
기본적으로 둘 다 Throwable을 상속받음.
**Error(에러):**
- java.lang.Error 클래스의 하위 클래스들임.
- 메모리 부족이나 심각한 시스템 오류와 같이 애플리케이션에서 복구가 불가능한 시스템 예외
- 예: 메모리 부족(OutOfMemoryError), 스택 오버플로우(StackOverflowError)
- JVM에서 발생시키기 때문에 애플리케이션 코드에서 잡아서는 안되며, 잡아서 대응할 수 있는 방법도 없음.
- 일반적으로 프로그램 종료로 이어짐
- 따라서 에러 처리를 하지 않아도 됨. (할 수도 없음)

**Exception(예외):**
- java.lang.Exception 클래스와 하위 클래스들임.
- 애플리케이션 코드에서 발생한 예외 상황.
- 예: 파일 없음, 네트워크 연결 실패, 잘못된 입력값 등
- 적절한 처리를 통해 프로그램 정상 실행 가능
- 개발자가 예측하고 대응할 수 있는 상황



## 2. Checked Exception vs Unchecked Exception (보길)

### 2.1 Checked Exception
- RuntimeException 클래스를 상속받지 않은 예외 클래스.
- 복구 가능성이 있는 예외이므로, 반드시 예외를 처리하는 코드를 함께 작성해야 한다.
- 반드시 예외 처리가 필요 (try-catch 또는 throws)
- 처리하지 않으면 컴파일 에러 발생
- 대표적인 예: IOException, SQLException
- 체크 예외는 개발자가 실수로 예외 처리를 누락하지 않도록 컴파일러가 도와준다. 하지만 개발자가 모든 체크 예외를 처리해주어야 하므로 번거로우며, 신경쓰지 않고 싶은 예외까지 처리해야 한다는 단점이 있다.
- 또한 애플리케이션 개발에서 발생하는 예외들은 복구 불가능한 경우가 많다. 예를 들어 SQLExceptoin과 같은 체크 예외를 catch해도, 쿼리를 수정하여 재배포하지 않는 이상 복구되지 않는다. 그래서 실제 개발에서는 대부분 언체크 예외를 사용한다.

**명시적 처리를 하지 않을 경우:**
```java
public void readFile(String path) {
    FileReader fr = new FileReader(path); // 컴파일 에러 발생!
    // Unhandled exception: java.io.FileNotFoundException
}
```

### 2.2 Unchecked Exception
- RuntimeException 클래스를 상속받는 예외 클래스.
- 복구 가능성이 없는 예외이므로, 컴파일러가 예외처리를 강제하지 않는다.
- 런타임 시점에서 확인되는 예외
- 명시적인 예외 처리는 선택사항


**명시적 처리를 하지 않을 경우:**
```java
public void divide(int a, int b) {
    int result = a / b; // 런타임에 예외 발생 가능
    // b가 0일 경우 ArithmeticException 발생하고 프로그램 중단
}
```

### 2.3 결론
- 체크 예외와 언체크 예외의 차이점은
- 반드시 잡아야 하는가 vs 안잡아도 되는가 의 차이...

## 3. RuntimeException 에 대하여 (재민)

RuntimeException은 프로그램의 실행 중(런타임)에 발생할 수 있는 예외들의 부모 클래스이다. (언체크 예외)

**주요 특징:**
1. 컴파일러가 예외 처리를 강제하지 않음
2. 프로그래머의 실수로 발생하는 경우가 대부분
3. 적절한 검사로 예방 가능

**대표적인 RuntimeException:**
1. NullPointerException: null 객체 참조
2. ArrayIndexOutOfBoundsException: 배열 범위 초과
3. ArithmeticException: 수학적 오류 (0으로 나누기 등)
4. ClassCastException: 잘못된 타입 캐스팅
5. IllegalArgumentException: 잘못된 인자 전달

**예방 방법:**
```java
// 나쁜 예
public void processArray(int[] arr) {
    int first = arr[0]; // arr이 null이거나 비어있으면 예외 발생
}

// 좋은 예
public void processArray(int[] arr) {
    if (arr == null || arr.length == 0) {
        throw new IllegalArgumentException("배열이 null이거나 비어있습니다.");
    }
    int first = arr[0];
}
```

## 4. NullPointerException 에 대하여 (민준)

NullPointerException은 Java에서 가장 흔히 발생하는 런타임 예외다.

**발생 원인:**
1. null 객체의 메서드 호출
2. null 객체의 필드 접근
3. null 배열의 길이 참조
4. null 객체의 배열 요소 접근
5. throw null 실행

**예방 방법:**
1. 객체 사용 전 null 체크
2. Optional 클래스 활용 (Java 8 이상)
3. 초기화 확실히 하기
4. 방어적 프로그래밍

```java
// 예방 예제
public class NullSafety {
    // 1. null 체크
    public void processString(String str) {
        if (str != null) {
            System.out.println(str.length());
        }
    }
    
    // 2. Optional 활용
    public void processOptional(String str) {
        Optional.ofNullable(str)
                .ifPresent(s -> System.out.println(s.length()));
    }
}
```

## 5. ArrayIndexOutOfBoundsException 에 대하여 (제영)

배열의 범위를 벗어난 인덱스에 접근할 때 발생하는 런타임 예외다

**발생 상황:**
1. 음수 인덱스 접근
2. 배열 길이보다 큰 인덱스 접근
3. 빈 배열 접근

**예방 방법:**
1. 배열 길이 확인
2. 반복문에서 올바른 범위 사용
3. 경계값 테스트

```java
// 안전한 배열 접근 예제
public class ArraySafety {
    public void processArray(int[] arr, int index) {
        // 1. null 체크
        if (arr == null) {
            throw new IllegalArgumentException("배열이 null입니다.");
        }
        
        // 2. 범위 체크
        if (index < 0 || index >= arr.length) {
            throw new IllegalArgumentException("유효하지 않은 인덱스: " + index);
        }
        
        // 안전한 접근
        int value = arr[index];
    }
}
```
**사용 예제(코테): 참고만 하세요..**
- 예외 발생을 원천 차단하기: https://www.acmicpc.net/source/93811007
- 예외가 발생하면 잡기: http://acmicpc.net/source/93811024

🔹 1. 성능 측면
try-catch는 자바에서 비싼 연산이야. 예외가 실제로 발생할 때마다 JVM은 스택 트레이스를 생성하고 예외 객체를 만들고, 예외 흐름으로 분기해
반면, if 조건문은 아주 가볍고 CPU 친화적이야.
테스트 데이터가 작을 땐 차이 없을 수도 있지만, 보통 예외가 많이 발생할수록 실행 시간과 메모리 사용량이 눈에 띄게 늘어나.

🔹 2. 코드 스타일 및 유지보수
예외 처리는 진짜 예외 상황에 쓰는 게 원칙이야 (예: null, 파일 열기 실패 등).
배열 경계를 넘는 걸 "정상적인 흐름"으로 처리하는 건, 코드 읽는 사람 입장에서 직관적이지 않고 위험해 보여.
동료 개발자나 리뷰어 입장에서는 "이 코드 뭐지?"라는 생각이 들 수 있어.

🔹 3. 코딩 테스트에서의 관점
네 말대로 실행 시간 차이가 거의 안 나는 경우도 많아. 작은 테스트 케이스에서는 try-catch도 통과할 수 있어.
하지만 일부 코딩 테스트 플랫폼(특히 실무 기반)은 예외 처리 남용을 감점 요소로 볼 수도 있어.


## 6. throws와 throw (예성)

### 6.1 throws
메서드에서 발생할 수 있는 예외를 선언하여 호출자에게 예외 처리를 위임한다. (아몰랑)

**주요 특징:**
1. 메서드 선언부에 사용
2. 여러 예외를 콤마로 구분하여 선언 가능
3. 예외 처리를 호출자에게 강제
4. 메서드의 예외 발생 가능성을 명시적으로 표현

### 6.2 throw
예외를 직접 발생시키는 키워드다.

**사용 상황:**
1. 특정 조건에서 의도적으로 예외 발생
2. 유효성 검사 실패 시
3. 비즈니스 로직상 예외 상황 표현

**throws vs throw:**
```java
// throws: 예외 선언
public void readFile() throws IOException {
    // 파일 읽기 작업
}
--> readFile()을 호출한 메서드에서 받은 예외를 처리해야함.
--> 그 메서드도 물론 예외를 던질 수 있음.

// throw: 예외 발생
public void validateAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("나이는 음수일 수 없습니다.");
    }
}
```

## 7. try, catch, finally (은수)

### 7.1 기본 구조
```java
try {
    // 예외가 발생할 수 있는 코드
} catch (예외타입1 e) {
    // 예외타입1에 대한 처리
} catch (예외타입2 e) {
    // 예외타입2에 대한 처리
} finally {
    // 항상 실행되는 코드
}
```

### 7.2 다중 catch 블록의 동작 방식

**예외 처리 순서:**
1. 예외 발생 시 첫 번째 catch 블록부터 순차적으로 검사
2. 발생한 예외와 일치하거나 상위 타입인 첫 번째 catch 블록이 실행
3. 하위 예외가 상위 예외보다 먼저 와야 함

**주의사항:**
1. 상위 예외를 먼저 캐치하면 하위 예외는 절대 실행되지 않음
2. 컴파일러가 순서 오류를 감지하여 에러 표시

```java
try {
    // 예외 발생 가능 코드
} catch (Exception e) {           // 잘못된 순서!
    // 모든 예외를 처리
} catch (NullPointerException e) { // 절대 실행되지 않음
    // NPE 처리
}

// 올바른 순서
try {
    // 예외 발생 가능 코드
} catch (NullPointerException e) { // 구체적인 예외
    // NPE 처리
} catch (Exception e) {           // 일반적인 예외
    // 기타 예외 처리
}
```

### 7.3 finally 블록
- 예외 발생 여부와 관계없이 항상 실행
- 주로 리소스 정리에 사용 (파일 닫기, 연결 종료 등)
- try-with-resources 구문으로 대체 가능 (Java 7 이상) -> 사용한 os의 자원을 finally구문에서 명시적으로 닫아주지 않아도 JVM에서 암시적으로 닫아준다.

```java
// try-with-resources 예제
try (FileReader fr = new FileReader("file.txt")) {
    // 파일 처리
} catch (IOException e) {
    // 예외 처리
} // 자동으로 fr.close() 호출
```

## 8. 사용자 정의 예외

### 8.1 사용자 정의 예외 클래스 생성
```java
// 사용자 정의 체크 예외
public class CustomCheckedException extends Exception {
    public CustomCheckedException(String message) {
        super(message);
    }
}

// 사용자 정의 언체크 예외
public class CustomUncheckedException extends RuntimeException {
    public CustomUncheckedException(String message) {
        super(message);
    }
}
```

### 8.2 사용자 정의 예외 사용
```java
public class CustomExceptionExample {
    public static void validateScore(int score) throws CustomCheckedException {
        if (score < 0 || score > 100) {
            throw new CustomCheckedException("점수는 0-100 사이여야 합니다.");
        }
    }
    
    public static void main(String[] args) {
        try {
            validateScore(150);
        } catch (CustomCheckedException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

### 9. 실습

1. 사용자 정의 체크 예외와 사용자 정의 언체크 예외를 각각 하나씩 만드세요. (MyCheckedException과 MyUncheckedException). 그리고 나서 두 예외를 던지는 메서드를 선언하고, 메서드 안에서 던지세요. 메인 메서드에서 두 메서드를 호출해보면서 체크 예외와 언체크 예외가 어떻게 다른지 주석으로 작성하세요.

2. 아래 코드는 NullPointerException이 발생한다. foo가 출력되도록 코드를 한 곳만 변경하세요.
```java
class Foo {
    public void foo() {
        System.out.println("foo");
    }
}

public class Main {
    public static void main(String[] args) {
        Foo f = null;

        try {
            f.foo();
        } catch (ArrayIndexOutOfBoundsException e) {
            f = new Foo();
        } finally {
            f.foo();
        }
    }
}
```

3. 아래 코드를 직접 작성하고, 실행하기 전에 실행 결과를 유추해 보세요.
```java
public class Main {
    public static void main(String[] args) {
        
        System.out.println(1);
        try {
            System.out.println(2);
            try {
                foo();
            } catch (RuntimeException e) {
                System.out.println(3);
            } catch (ExA e) {
                System.out.println(4);
                throw new ExB();
            } finally {
                System.out.println(6);
            }
        } catch (ExB e) {
            System.out.println(7);
        } catch (Throwable e) {
            System.out.println(8);
        } finally {
            System.out.println(9);
        }
    }

    private static void foo() throws ExA {
        throw new ExA();
    }
}

class ExA extends Exception {

}

class ExB extends RuntimeException {

}
```

4. 프로그래머스 콜라 문제(https://school.programmers.co.kr/learn/courses/30/lessons/132267) 를 자바로 풀어보세요.

5. 1,2,3,4를 .java나 .txt 파일로 붙여넣어서 본인의 학습노트에 올리고 커밋 후 원격 리포지토리에 푸시하세요.


### 세션 배부
- 제네릭 타입       - 예성
- 제네릭 메서드     - 보길
- 타입 추론        - 민준
- 와일드카드        - 동건
- 타입 이레이저

Collection
├── List
│   ├── ArrayList
│   └── LinkedList
├── Set
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue / Deque
    ├── LinkedList
    ├── ArrayDeque
    ├── PriorityQueue
    └── BlockingQueue 등...

Map
├── HashMap
├── LinkedHashMap
├── TreeMap
├── Hashtable
└── ConcurrentHashMap