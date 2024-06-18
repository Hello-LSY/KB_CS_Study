## AOP에 대해 설명해 주세요.

AOP는 Aspect Oriented Programming, 즉 관점 지향 프로그래밍을 의미합니다. 

AOP의 목표는 프로그램의 핵심 비즈니스 로직과 횡단 관심사(Cross-Cutting Concerns)를 분리하는 것입니다. 

횡단 관심사에는 로깅, 보안, 트랜잭션 관리 등이 포함됩니다. AOP를 통해 이러한 공통 기능을 별도의 모듈로 분리하여 코드의 중복을 줄이고, 핵심 비즈니스 로직을 보다 명확하게 유지할 수 있습니다. 

Spring AOP는 프록시 기반의 AOP 구현을 제공하며, 주로 @AspectJ 어노테이션 스타일을 사용합니다.

### @Aspect는 어떻게 동작하나요?

@Aspect 어노테이션은 Spring AOP에서 사용하는 어노테이션으로, 해당 클래스가 Aspect임을 명시합니다. Aspect는 여러 가지 Advice와 Pointcut으로 구성됩니다.

Advice에는 특정 Join Point에서 실행될 코드를 정의합니다. 예를 들어, 메서드 실행 전후, 예외 발생 시 등의 상황에 실행될 코드를 말합니다. @Before, @After, @Around, @AfterReturning, @AfterThrowing 등이 있습니다.

Pointcut에는 Advice가 적용될 Join Point를 정의하는 표현식입니다. 주로 메서드 이름 패턴이나 어노테이션을 기반으로 설정합니다.