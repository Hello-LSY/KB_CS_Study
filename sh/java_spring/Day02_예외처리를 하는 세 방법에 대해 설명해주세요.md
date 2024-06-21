6-1. 예외처리를 하는 세 방법

1.try-catch 블록: 예외가 발생할 수 있는 코드를 try 블록에 작성하고, 예외가 발생했을 때 이를 처리할 코드를 catch 블록에 작성합니다.

//
try {
    // 예외가 발생할 수 있는 코드
} catch (ExceptionType e) {
    // 예외 처리 코드
}

//
2.throws 키워드: 메서드 선언부에 throws 키워드를 사용하여 해당 메서드에서 발생할 수 있는 예외를 명시하고, 메서드를 호출하는 쪽에서 예외를 처리하게 합니다.

//
public void myMethod() throws ExceptionType {
    // 예외가 발생할 수 있는 코드
}
//

3.try-with-resources: 자원을 자동으로 관리하면서 예외를 처리할 수 있는 구조입니다. AutoCloseable을 구현한 자원을 사용한 후 자동으로 닫습니다.
//
try (ResourceType resource = new ResourceType()) {
    // 자원 사용 및 예외 발생 가능 코드
} catch (ExceptionType e) {
    // 예외 처리 코드
}
//


6-2. CheckedException, UncheckedException의 차이

CheckedException: 컴파일 타임에 체크되는 예외로, 반드시 처리(try-catch)하거나 메서드 선언부에 throws 키워드를 사용해 선언해야 합니다. 주로 외부 자원 접근, 파일 I/O, 네트워크 통신과 관련된 예외들이 이에 해당합니다. 예를 들어, IOException, SQLException 등이 있습니다.

//
public void readFile() throws IOException {
    // 파일 읽기 코드
}
//

UncheckedException: 런타임에 발생하는 예외로, 명시적으로 처리하지 않아도 됩니다. 주로 프로그래머의 실수로 인해 발생하는 논리적 오류가 이에 해당합니다. RuntimeException을 상속하는 예외들이 이에 속하며, 예를 들어, NullPointerException, ArrayIndexOutOfBoundsException 등이 있습니다.

//
UncheckedException: 런타임에 발생하는 예외로, 명시적으로 처리하지 않아도 됩니다. 주로 프로그래머의 실수로 인해 발생하는 논리적 오류가 이에 해당합니다. RuntimeException을 상속하는 예외들이 이에 속하며, 예를 들어, NullPointerException, ArrayIndexOutOfBoundsException 등이 있습니다.

//

6-3. 예외처리가 성능에 큰 영향을 미치나요?
예외처리는 일반적인 코드 흐름과 다르게 실행되는 경우가 많기 때문에 성능에 영향을 미칠 수 있습니다. 특히, 예외가 자주 발생하는 경우 성능 저하가 눈에 띄게 나타날 수 있습니다. 예외 발생 자체는 비용이 높은 연산이기 때문입니다.

부하를 줄이는 방법:

1.예외 발생을 최소화: 예외를 사용하는 대신, 조건문을 사용하여 예외 발생을 방지합니다.
//
if (b != 0) {
    result = a / b;
} else {
    // 예외 처리
}
//
2.필요한 경우에만 예외 사용: 예외를 남용하지 않고, 정말 필요한 경우에만 사용합니다.
3.적절한 예외 처리: 불필요한 예외 처리를 피하고, 정확하게 예외를 처리합니다.

