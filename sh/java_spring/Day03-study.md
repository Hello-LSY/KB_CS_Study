9. Java의 GC에 대해 설명해 주세요.

- GC는 JVM의 heap영역에 할당한 메모리 영역 중 사용하지 않는 영역을 탐지하여 해제(제거한다.)
- jvm의 heap 영역의 구조부터 알아야 한다.
    - young Generation: 새롭게 생성하는 객체들이 존재하는 곳
    - Old Generation: Young Generation영역에서 오래동안 살아남은 객체들이 존재하는 곳
    - Meta space: 클래스의 메타데이터, 문자열 정보를 저장하는 영역(자바 8이후 생김)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b8f02756-93e8-4d07-9515-e8736cbeff65/8f918f14-f700-4106-ae6c-9e128652d6c2/Untitled.png)

- 근데Young영역을 또 3가지 영역으로 나눈다.
    - Eden: new를 통해 새로 생성된 객체의 위치
    - Suvivor:
        - 최소 1번의 GC 이상 살아남은 객체가 존재하는 위치
        - 0또는 1 구역 둘중 하나는 꼭 비어 있어야 한다.

- **GC 청소 방법**
    - Reference Counting: Root Space에 접근할 수 있는 방법이 없는 객체는 삭제 대상이 된다.
- Mark-Swwep-Compace

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b8f02756-93e8-4d07-9515-e8736cbeff65/cfef2a2a-7546-4381-a5a7-ec71357a8763/Untitled.png)

- JVM에서 사용하는 방식
- Root Space에서부터 순환을 하여 방문한 객체에 Mark한다.
- Root Space는 stack, method stack,method 영역을 뜻한다.
- Mark되지 않은 객체는 삭제(sweep)대상이 된다.

- **GC 단점**:
    - 메모리가 언제 해제되는지 정확하게 알 수 없기 때문에 제어하기 어려움
    - GC가 동작하는 동안에는 다른 동작을 멈추기 때문에 오버헤드가 발생하는 문제가 생긴다
    
       →Stop-The-World 이라고도 한다. 이 시간을 최소화시키는 것이 중요
    
- GC는 어떤 Object를 가비지로 판단하는 걸까?
    - 객체에 레퍼런스가 있담면 Reachable, 없다면 Unreachable

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b8f02756-93e8-4d07-9515-e8736cbeff65/654b97ba-0a02-4124-aa87-08c9c67054ab/Untitled.png)

- MInor GC와 Major GC의 차이점 (동작법은 블로그 글 참고)

[☕ 가비지 컬렉션 동작 원리 & GC 종류 💯 총정리](https://inpa.tistory.com/entry/JAVA-☕-가비지-컬렉션GC-동작-원리-알고리즘-💯-총정리)

9-1.finalize() 를 수동으로 호출하는 것은 왜 문제가 될 수 있을까요?

- finalize란 객체가 GC에 의해 수집되기 직전에 호출되는 메서드이다.

- 일단 finalize는 사용하지 않기를 권고한다.
- finalize()메서드의 호출 시점이 불확실하다. (명시해도 즉시 제거되지 않을 수 있다.)
- finalize()메서드를 사용하여 자원 해제를 수행하기 보다는,try-fianlly 블록 등을 사용하여 명시적으로 자원을 해제하는 것이 좋다.
- java9부터는 AutoCloseabel나 try-with-resources 문법을 사용하는 것을 권장

9-2.어떤 변수의 값이 null이 되었다면, 이 값은 GC가 될 가능성이 있을까요?

- 값이 null인 변수는 해당 변수가 참조하던 객체를 더 이상 참조하지 않기 때문에 GC의 대상이 될 수 있음.
- 하지만 값이 null인 변수가 GC의 대상이 될 수 있는지는 해당 변수를 참조하는 다른 변수나 객체의 참조 상황에 따라 달라질 수 있다.

10-1. equals()와 hashcode()에 대해 설명해 주세요.

- **equals()란?** 두 참조 변수의 값이 같은지 다른지 동등 여부를 비교할 때 사용
    - 문자열이 아니고 객체 데이터라면? **항상 주소를 비교한다고 생각하자**
    
    ```java
    class Person {
        String name;
    
        public Person(String name) {
            this.name = name;
        }
    
    public class Example {
        public static void main(String[] args) {
            Person person1 = new Person("홍길동");
            Person person2 = new Person("홍길동");
    
            System.out.println(person1 == person2); // == 은 객체타입인경우 주소값을 비교한다. 서로다른 객체는 다른 주소를 가지고 있기 때문에 false가 출력됨
    
            System.out.println(person1.equals(person2)) // equals또한 객체타입인경우 주소값을 비교하기 때문에 false가 출력된다.
        }
    }
    ```
    
    - euqals 오버라이딩: 위의 두 객체 변수는 힙 영역에 따로 저장됨 (컴퓨터적인 관점)
    →사용 입장에서는 두객체는 어찌보면 같은 데이터다
    →오바라이딩 해서 equals를 데이터 값으로 비교하게 하자!
- **hashCode?** 객체의 주소 값을 이용해서 해싱 기법을 통해 해시 코드를 만든 후 반환한다.
    - 해시코드는 객체의 지문이라고도 한다.
    - 해시코드는 주소값은 아니고, 주소값으로 만든 고유한 숫자값
    
    ```java
    class Person {
        String name;
    
        public Person(String name) {
            this.name = name;
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Person p1 = new Person("홍길동");
            Person p2 = new Person("홍길동");
    
            // 객체 인스턴스마다 각기 다른 주해시코드(주소))를 가지고 있다.
            System.out.println(p1.hashCode()); // 622488023
            System.out.println(p2.hashCode()); // 1933863327
        }
    }
    ```
    

- 실제 object 클래스에 정의된 hashCode() 메서드 정의를 보면 다음과 같이 정의된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b8f02756-93e8-4d07-9515-e8736cbeff65/c23da09e-1525-4e24-9d35-dae33ac34b70/Untitled.png)

- JNI(Java Native Interface)는 native코드를 JVM에 적재시키고 실행해주는 머신
→hashcode()도 native코드
→추상메서드 처럼 정의 되어있다.
- hashCode 오버라이딩
    - equals()를 오바라이딩으로 재정의 하면 hashcode도 같이 재정의 해줘야 한다.
    
    →hash값을 사용하는 Collection Framework(HashSet,HashMap,HashTable)을 사용할 때 문제가 발생하기 때문이다.
    

10-2본인이 hashcode() 를 정의해야 한다면, 어떤 점을 염두에 두고 구현할 것 같으세요?

10-3그렇다면 equals() 를 재정의 해야 할 때, 어떤 점을 염두에 두어야 하는지 설명해 주세요.

11. IoC와 DI에 대해 설명해 주세요.

11-1.후보 없이 특정 기능을 하는 클래스가 딱 한 개하면, 구체 클래스를 그냥 사용해도 되지 않나요? 그럼에도 불구하고 왜 Spring에선 Bean을 사용 할까요?
11-2.Spring의 Bean 생성 주기에 대해 설명해 주세요.
11-3.프로토타입 빈은 무엇인가요?

12.AOP에 대해 설명해 주세요.

12.@Aspect는 어떻게 동작하나요?

- 
    -