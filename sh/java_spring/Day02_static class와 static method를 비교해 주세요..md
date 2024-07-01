1. static class와 static method를 비교해 주세요.

//
//Static class
public class OuterClass {
    static class StaticNestedClass {
        static void staticMethod() {
            System.out.println("Static method in static nested class");
        }
    }
}
//
- 정의: ‘static class’ 는 주로 클래스 내부에 선언되는 정적 중첩 클래스(nested class)로, 외부 클래스의 인스턴스와 독립적으로 사용됩니다.
- 특징:
    - 인스턴스를 생성할 수 없음
    - 외부 클래스의 인스턴스 멤버 접근x
    - 정적 변수 및 메서드만 가질 수 있음.


//
    //Static Method
public class ExampleClass {
    static void staticMethod() {
        System.out.println("Static method in ExampleClass");
    }
}
//

- 정의: ‘static method’는 특정 클래스에 속한 메서드로, 인스턴스를 생성하지 않고 클래스 이름으로 직접 호출할 수 있다.
- 특징
    - 인스턴스 멤버에 접근 x, 정적 변수나 메서드에만 접근할 수 있다.
    - 클래스의 인스턴스와 독립적으로 호출할 수 있다.

    
5-1.static을 사용하면 어떤 이점을 얻을 수 있나요? 어떤 제약이 걸릴까요?

- static 은  프로그래밍에서 정적 멤버(변수,메서드,클래스 등)을 정의할 때 사용한다. →클래스 자체에 속함을 의미한다.
- 이점
    - 메모리 효율성:
        - static 변수는 클래스 수준에서 하나만 존재한다.
        - 여러 인스턴스가 공유할 필요가 있는 데이터를 효율적으로 저장
    - 편리한 접근성
        - 인스턴스 생성 안해도 클래스 이름으로 접근 가능
    - 상태 공유:
        - 인스턴스 간에 상태를 쉽게 공유할 수 있다.
- 제약
    - 인스턴스 멤버 접근 불가:
        - 인스턴스→static은 가능한데 static→인스턴스는 불가능(클래스 수준에서 존재하기 때문)
    - 메모리 누수 위험:
        - 클래스가 메모리에 로드되는 동안 계속 유지됨→ 큰 데이터를 static으로 관리하면 문제될 수 있음
    - 테스트 어려움:
        - 테스트 간에 상태가 공유되어 단위 테스트에서 상태를 초기화하기 어려움
    - 객체 지향 원칙 위반 가능성
        - 객체 지향 프로그래밍의 핵심원칙인 캡슐화와 객체 간의 명확한 경계를 흐릴 수 있다.
        
        !!핵심원칙 찾아보기 캡슐화가 무엇일까?
        

5-2. 컴파일 과정에서 static이 어떻게 처리되는지 설명해주세요

컴파일 과정에서 static의 처리
클래스 로딩:

JVM은 클래스를 처음으로 참조할 때 해당 클래스를 메모리에 로드합니다.
클래스 로딩은 클래스의 바이트 코드를 JVM의 메모리에 로드하는 과정입니다.
클래스 초기화:

클래스가 로드되면, JVM은 클래스 초기화 과정에서 static 변수와 static 블록을 실행합니다.
클래스 초기화는 클래스가 처음 로드될 때 한 번만 수행됩니다.
메모리 할당:

static 변수는 클래스 로딩 시점에 메모리에 할당됩니다.
static 변수는 모든 인스턴스가 공유하는 클래스 레벨의 변수로, 메서드 영역(Method Area)에 저장됩니다.
바이트코드 생성:

자바 컴파일러는 자바 소스 코드를 컴파일하여 바이트코드를 생성합니다.
이 과정에서 static 키워드를 가진 변수나 메서드는 클래스 레벨에서 접근할 수 있도록 바이트코드로 변환됩니다.

//
public class Example {
    static int staticVariable;
    int instanceVariable;

    static {
        staticVariable = 10;
    }

    public static void staticMethod() {
        System.out.println("Static method called");
    }
    
    public void instanceMethod() {
        System.out.println("Instance method called");
    }
}

//
바이트코드 변환 과정
1.static 변수 초기화:

클래스가 로드될 때 static 변수는 초기화됩니다.
위 예시에서 staticVariable은 클래스가 로드될 때 초기값 10으로 설정됩니다.

2.static 블록 실행:

static 블록은 클래스 초기화 시점에 실행됩니다.
static 블록은 클래스 로딩 후 한 번만 실행되므로, staticVariable은 10으로 설정됩니다.

3.static 메서드:

staticMethod는 인스턴스 생성 없이 클래스명으로 호출할 수 있습니다.
바이트코드에서 static 메서드는 클래스 레벨에서 호출할 수 있도록 참조됩니다