## 리플렉션에 대해 설명해 주세요.

__Reflection__<br/>
구체적 타입을 알지 못해도 런타임에 그 클래스의 정보(메서드, 타입, 변수 등)에 접근할 수 있게 해주는 자바 API입니다. 스프링이 실행 시점에 빈을 주입할 수 있는 것과 JPA의 Entity나 Response DTO가 기본 생성자를 꼭 가져야 하는 이유이기도 합니다. <br/>

Reflection은 JVM의 메모리 영역에 저장된 클래스 데이터에서 필요한 정보들을 꺼내옵니다.

다음 코드를 살펴봅시다.

```java
// 자바의 다형성 덕분에 Object 객체 생성은 가능하다.
public static void main(String[] args) {
	Object obj = new Car("foo", 0);
	obj.move(); // Car의 move() 메서드를 사용할 수 없다, 컴파일 에러!
}
```
생성된 obj 라는 객체는 Object라는 클래스만 알 뿐 Car 클래스라는 구체적인 타입은 모릅니다. 이것을 가능하게 해주는 기능이 Relection입니다.

```java
public static void main(String[] args) throws Exception {
    Object obj = new Car("foo", 0);
    Class carClass = Car.class;

    Method move = carClass.getMethod("move");

    // move 메서드 실행, invoke(메서드를 실행시킬 객체, 해당 메서드에 넘길 인자)
    move.invoke(obj, null);

    Method getPosition = carClass.getMethod("getPosition");
    int position = (int)getPosition.invoke(obj, null);

    System.out.println(position);
    // 출력 결과: 1
}
```
Reflection API로 클래스 타입 Car를 알지 못해도 obj가 move 메서드에 접근 가능합니다.


### 의미만 들어보면 리플렉션은 보안적인 문제가 있을 가능성이 있어보이는데, 실제로 그렇게 생각하시나요? 만약 그렇다면, 어떻게 방지할 수 있을까요?
있습니다. <br/>
개발자가 비공개하고자 하는 필드나 메서드에 접근 가능하기 때문에 의도하지 않은 방식으로 프로그램을 동작하게 만들 수 있습니다. <br/>
이는 보안 관리자를 설정하여 방지할 수 있습니다. 자바 애플리케이션에 보안 관리자를 설정하여 Reflection을 통해 민감한 정보에 접근하는 것을 방지할 수 있습니다.
```java
System.setSecurityManager(new SecurityManager());
```

### 리플렉션을 언제 활용할 수 있을까요?
스프링이 실행 시점에 빈을 주입할 때와 개발 시 dto를 직렬화 또는 역직렬화할 때 사용 가능합니다.
