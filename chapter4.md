# Chapter 4: Generic

## 1. 제네릭 타입

제네릭(Generic)은 Java 5부터 도입된 기능으로, 클래스나 메서드에서 사용할 데이터 타입을 `컴파일 시`에 미리 지정하는 방법.

제네릭의 장점  
- 타입 안정성 보장
- 불필요한 타입 변환 제거
- 코드의 재사용성 증가 <- 제일 큰 장점.

### 제네릭 클래스 선언

제네릭 클래스는 다음과 같이 사용한다

```java
public class Box<T> {
    private T item;
    
    public void setItem(T item) {
        this.item = item;
    }
    
    public T getItem() {
        return item;
    }
}
```

여기서 `T`는 타입 파라미터로, 실제 클래스를 사용할 때 구체적인 타입으로 대체된다.   
이렇게 쓴다면, IntegerBox, CharacterBox, DoubleBox, StringBox등의 클래스를 모두 따로 생성하는 짓을 안해도 됨

### 제네릭 클래스 사용 예제

```java
Box<String> stringBox = new Box<String>();
stringBox.setItem("Hello Generic!");
String str = stringBox.getItem();

Box<Integer> intBox = new Box<Integer>();
intBox.setItem(100);
int value = intBox.getItem();
```

### 제네릭 타입 제한
타입 파라미터에 제한을 두어 특정 타입의 자식 클래스만 대입할 수 있게 할 수 있음.  
(상속 관계 이용)

```java
public class NumberBox<T extends Number> {
    private T number;
    
    public void set(T number) {
        this.number = number;
    }
    
    public T get() {
        return number;
    }
}
```

## 2. 제네릭 메서드

제네릭 메서드는 메서드 단위로 타입 파라미터를 사용하는 방법이다.  
클래스에서 정의한 제네릭 타입과는 전혀 별개로 동작하며, `메서드마다 다른 제네릭 타입을 사용할 수 있다.`

### 제네릭 메서드의 특징
- 메서드 내에서만 사용할 수 있는 독립적인 제네릭 타입을 선언
- 메서드의 리턴 타입 앞에 타입 파라미터를 선언
- static 메서드에서도 사용 가능

### 기본적인 제네릭 메서드 예제

```java
class MyClass {
    // 가장 단순한 형태의 제네릭 메서드
    public static <T> void printItem(T item) {
        System.out.println(item);
    }
    
    // 배열을 출력하는 제네릭 메서드
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }
}

// 사용 예제
public class Main {
    public static void main(String[] args) {
        // 다양한 타입으로 메서드 호출
        MyClass.printItem("Hello");  // String 타입
        MyClass.printItem(100);      // Integer 타입
        MyClass.printItem(3.14);     // Double 타입
        
        // 배열 출력
        String[] strings = {"사과", "바나나", "오렌지"};
        Integer[] numbers = {1, 2, 3, 4, 5};
        
        MyClass.printArray(strings);  // 사과 바나나 오렌지
        MyClass.printArray(numbers);  // 1 2 3 4 5
    }
}

// 이런 것도 가능 : 리턴 타입을 T로 설정
public class Main {
    public static void main(String[] args) {
        String s = foo("hi");
        System.out.println(s);

        Integer i = foo(3);
        System.out.println(i);
    }

    public static <T> T foo(T item) {
        System.out.println("received : " + item);
        return item;
    }
}
```

### 제네릭 메서드의 타입 제한

타입 파라미터에 제한을 두어 특정 타입과 그 자식 타입만 사용할 수 있게 할 수 있다.

```java
class Calculator {
    // Number 클래스와 그 자식 클래스만 사용 가능
    public static <T extends Number> double doSum(T[] array) {
        double sum = 0.0;
        for (T element : array) {
            sum += element.doubleValue();
        }
        return sum;
    }
}

```

## 제네릭 메서드 사용 시 주의사항

1. 타입 파라미터는 static 필드로 사용할 수 없다
   - static은 인스턴스 생성 전에 사용할 수 있어야 하는데, 제네릭 타입은 인스턴스 생성 시에 결정되므로 충돌이 발생

2. 제네릭 타입의 배열은 직접 생성할 수 없다
   - 이유: 자바의 배열은 런타임에 타입 정보를 보관하는데, 제네릭은 컴파일 타임에만 타입을 검사하고 런타임에는 타입 정보가 소거되기 때문
   - 대안: ArrayList<T>를 사용하거나, Object 배열을 생성한 후 타입 캐스팅하여 사용

3. 기본 타입(primitive type)은 직접 사용할 수 없다
   - 이유: 제네릭은 `참조 타입`만 사용 가능하도록 설계되었기 때문
   - 대안: 래퍼 클래스(Integer, Double, Boolean 등)를 사용해야 함
   ```java
   // 불가능한 예
   ArrayList<int> nono = new ArrayList<int>(); // int나 double 등의 기본 타입으로 T를 지정할 수 없음
   
   // 가능한 예
   ArrayList<Integer> list = new ArrayList<Integer>();  // int 대신 Integer 사용
   ArrayList<Double> numbers = new ArrayList<Double>(); // double 대신 Double 사용
   ```

4. instanceof 연산자와 함께 사용할 수 없다
   - 이유: 타입 소거(Type Erasure) 때문. 컴파일 후에는 제네릭 타입 정보가 제거되어 런타임에 타입을 확인할 수 없음
   - 대안: Class 객체를 파라미터로 전달받아 타입을 확인
   ```java
   // 불가능한 예
   public static <T> boolean checkType(Object obj) {
       if (obj instanceof T) {  // 컴파일 에러
           return true;
       }
       return false;
   }
   
   // 가능한 예
   public static <T> boolean checkType(Object obj, Class<T> type) {
       return type.isInstance(obj);
   }
   ```

5. 제네릭 타입의 catch 블록을 만들 수 없다
   - 이유: 예외 처리는 런타임에 발생하는데, 제네릭은 컴파일 타임에만 유효하기 때문
   ```java
   // 불가능한 예
   public static <T extends Exception> void process() {
       try {
           // 코드
       } catch (T e) {  // 컴파일 에러
           // 예외 처리
       }
   }
   ```

이러한 제약사항들은 대부분 자바의 타입 소거 메커니즘과 관련이 있습니다. 타입 소거는 제네릭을 구현하면서 이전 버전과의 호환성을 유지하기 위해 도입된 방식이기 때문에, 이로 인한 몇 가지 제약사항들을 이해하고 적절한 대안을 사용하는 것이 중요합니다.

## 3. 타입 추론

타입 추론은 컴파일러가 주어진 정보를 바탕으로 제네릭의 실제 타입을 추론하는 기능이다.   
Java 7부터는 다이아몬드 연산자(<>)를 통해 타입 추론이 더욱 간편해졌다.

### 타입 추론 예제

```java
// Java 7 이전
Map<String, List<String>> myMap = new HashMap<String, List<String>>();

// Java 7 이후 (다이아몬드 연산자 사용)
Map<String, List<String>> myMap = new HashMap<>();

// 메서드 타입 추론
List<String> list = Arrays.asList("a", "b", "c");
```

## 4. 와일드카드

와일드카드(?)는 알 수 없는 타입을 나타내며, 유연한 제네릭 프로그래밍을 가능하게 한다.  
메서드의 매개변수(파라미터)에 사용한다.

### 상한 와일드카드 (Upper Bounded Wildcard)
`<? extends Type>`은 Type이나 그 자식 클래스만 허용합니다.

```java
public static double sumOfList(List<? extends Number> list) {
    double sum = 0.0;
    for (Number num : list) {
        sum += num.doubleValue();
    }
    return sum;
}

// 사용 예제
List<Integer> integers = Arrays.asList(1, 2, 3);
List<Double> doubles = Arrays.asList(1.1, 2.2, 3.3);
System.out.println(sumOfList(integers)); // 6.0
System.out.println(sumOfList(doubles));  // 6.6
```

### 하한 와일드카드 (Lower Bounded Wildcard)
`<? super Type>`은 Type이나 그 부모 클래스만 허용합니다.

```java
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 5; i++) {
        list.add(i);
    }
}
```

### 언바운드 와일드카드 (Unbounded Wildcard)
`<?>`는 모든 타입을 허용합니다.

```java
public static void printList(List<?> list) {
    for (Object elem : list) {
        System.out.print(elem + " "); // toString이 오버라이드되어 있어야 한다. 구현되어있지 않다면 제일 상위의 Object.toString() 이 호출됨..
    }
    System.out.println();
}
```

## 5. 타입 이레이저
> 자바는 컴파일 시점에 제네릭을 사용한 코드에 문제가 없는지 완벽하게 검증한다

타입 이레이저(Type Erasure)는 제네릭이 `컴파일 타임에만 타입 검사`를 하고, `런타임에는 타입 정보가 제거`되는 것을 의미함.  
이는 Java가 제네릭을 구현하면서 이전 버전과의 호환성을 유지하기 위해 도입한 방식이다.

### 타입 이레이저의 동작 방식

1. 제네릭 타입의 경계(bound)를 제거합니다.
2. 타입 파라미터를 제거하고 경계 타입이나 Object로 대체합니다.
3. 필요한 곳에 타입 캐스팅을 추가합니다.

예를 들어, 다음 코드는:

```java
public class Box<T> {
    private T data;
    public void set(T data) { this.data = data; }
    public T get() { return data; }
}
```

컴파일 후에는 다음과 같이 변환됩니다 (상한 제한 없이 선언한 T는 Object 로 변환됨.
타입 매개변수 제한을 걸면 제한된 타입으로 변환됨.):
> 컴파일: .java -> .class

```java
public class Box {
    private Object data;
    public void set(Object data) { this.data = data; }
    public Object get() { return data; }
}
```

### 타입 이레이저의 제약사항

- 제네릭 타입의 instanceof 연산을 사용할 수 없습니다.  
- 컴파일 이후에는 제네릭 타입 정보가 존재하지 않음 ~.class로 자바를 실행하는 런타임에는 우리가 지정한 타입 정보가 모두 제거됨  
- 따라서 런타임에 타입을 활용하는 코드는 작성할 수 없음

```java
// 컴파일 에러 예제
public class GenericType<T> {
    // 컴파일 에러: static 필드에 타입 파라미터 사용 불가
    private static T instance;
    
    // 컴파일 에러: 제네릭 배열 생성 불가
    private T[] array = new T[10];
}
```

### 타입 이레이저 장점
타입 이레이저는 런타임에 제네릭 타입 정보를 유지할 필요가 없도록 하여, 런타임 오버헤드를 줄인다. 이는 성능 최적화에 도움이 된다.


## 6. 실습

### 1. 제네릭 리스트 구현
아래 예제는 자바 컬렉션 프레임워크의 ArrayList와 LinkedList를 모방해서 만든 예제입니다. 직접 따라 쳐 보고, main 메서드에서 사용해 보세요.

```java
import java.util.*;

public class Generic1 {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        System.out.println(list.get(2));

        // 내가 만든 ArrayList 사용해보기

        // 내가 만든 LinkedList 사용해보기
    }
}

// 기본 리스트 인터페이스
interface MyList<T> {
    void add(T element);
    T get(int index);
    int size();
    boolean isEmpty();
}

// ArrayList 구현
class MyArrayList<T> implements MyList<T> {
    private static final int DEFAULT_CAPACITY = 10;
    private Object[] elements;
    private int size;

    public MyArrayList() {
        elements = new Object[DEFAULT_CAPACITY];
        size = 0;
    }

    @Override
    public void add(T element) {
        if (size == elements.length) {
            // 배열이 가득 차면 크기를 2배로 늘림
            Object[] newElements = new Object[elements.length * 2];
            System.arraycopy(elements, 0, newElements, 0, size);
            elements = newElements;
        }
        elements[size++] = element;
    }

    @SuppressWarnings("unchecked")
    @Override
    public T get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index: " + index + ", Size: " + size);
        }
        return (T) elements[index];
    }

    @Override
    public int size() {
        return size;
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }
}

// LinkedList 구현
class MyLinkedList<T> implements MyList<T> {
    private class Node {
        T data;
        Node next;

        Node(T data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node head;
    private int size;

    public MyLinkedList() {
        head = null;
        size = 0;
    }

    @Override
    public void add(T element) {
        Node newNode = new Node(element);
        if (head == null) {
            head = newNode;
        } else {
            Node current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
        size++;
    }

    @Override
    public T get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index: " + index + ", Size: " + size);
        }

        Node current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.data;
    }

    @Override
    public int size() {
        return size;
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }
}
```

### 2. 시저 암호 문제
프로그래머스 시저 암호 문제(https://school.programmers.co.kr/learn/courses/30/lessons/12926)를 푸세요.

### 3. 1번과 2번을 파일로 저장한 후 본인의 학습노트 리포지토리에 commit 후 push 하세요.


---
## 세션 배부
컬렉션 프레임워크
- List
- Set
- Queue
- Deque
- Map


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