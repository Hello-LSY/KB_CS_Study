### JVM이 정확히 무엇이고, 어떤 기능을 하는지 설명해 주세요.

꼬리질문
1. 하나의 JVM 구현체로 플랫폼 독립성을 실현할 수 있나요?

2. JVM이 어떻게 자동으로 메모리를 관리하나요? (~를 사용해서 관리합니다 >>> 단답형으로 대답할 시 어떻게 동작하는가에 대해 추가 질문)



<개념>

JVM (Java Virtual Machine)

java 프로그램을 실행하기 위한 가상머신

java 소스 코드는 바이트코드로 컴파일되며, JVM은 바이트코드를 해석하고 실행



<기능>

- java의 플랫폼 독립성 실현 : JVM은 자바 프로그램이 OS나 HW에 종속되지 않고 다양한 환경에서 실행될 수 있도록 함.

- 메모리 자동 관리 : GC는 트레이싱 알고리즘을 사용하여 메모리를 자동으로 관리해줌.

- 클래스 파일 로드

- 로드된 바이트코드 유효성 확인

- jvm 인터프리터나 JIT 컴파일러가 바이트코드를 기계어로 변환