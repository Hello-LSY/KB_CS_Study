
8/. Java Stream에 대해 설명해 주세요

Java Stream은 컬렉션, 배열, 또는 I/O 채널의 요소를 일관된 방식으로 처리할 수 있는 API입니다. Stream API는 함수형 프로그래밍 스타일을 사용하여 선언적 방식으로 데이터를 처리할 수 있도록 합니다. 주요 연산에는 필터링, 매핑, 정렬, 집계 등이 포함됩니다.

8-1. Stream과 for-loop의 성능 차이를 비교해 주세요
Stream과 for-loop의 성능 차이는 경우에 따라 다릅니다. 일반적으로 for-loop은 낮은 수준의 제어와 단순한 연산에서 더 빠를 수 있지만, Stream은 코드의 가독성을 높이고 유지 보수를 용이하게 합니다. Stream의 경우, 내부적으로 최적화된 알고리즘을 사용하여 효율적으로 처리할 수 있지만, 일부 상황에서는 오버헤드가 발생할 수 있습니다.

8-2. Stream과 병렬처리 할 수 있나요?
Java Stream API는 병렬 처리를 지원합니다. parallelStream() 메서드를 사용하여 병렬 Stream을 생성할 수 있으며, 내부적으로 포크-조인(Fork-Join) 프레임워크를 사용하여 병렬 처리를 수행합니다.
//
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.parallelStream().reduce(0, Integer::sum);
//
8-3. Stream에서 사용할 수 있는 함수형 인터페이스에 대해 설명해주세요
Stream에서 사용할 수 있는 주요 함수형 인터페이스는 다음과 같습니다:

Function<T, R>: 입력을 받아 출력으로 매핑하는 함수
Predicate<T>: 입력을 받아 boolean 값을 반환하는 함수
Consumer<T>: 입력을 받아 처리하지만, 값을 반환하지 않는 함수
Supplier<T>: 값을 제공하는 함수
BinaryOperator<T>: 두 입력을 받아 하나의 결과를 반환하는 함수 (예: reduce 연산)
UnaryOperator<T>: 하나의 입력을 받아 같은 타입의 결과를 반환하는 함수

8-4. 가끔 외부 변수를 사용할 때, final 키워드를 붙여서 사용하는데 왜 그럴까요? 꼭 그래야 할까요?
Stream 내부에서 외부 변수를 사용할 때, 해당 변수는 사실상 final이어야 합니다. 즉, 해당 변수는 값을 한 번만 설정할 수 있으며 이후 변경되지 않아야 합니다. 이는 람다 표현식이나 익명 클래스에서 외부 변수를 안전하게 사용할 수 있도록 보장합니다. 따라서 final 키워드를 명시적으로 붙이는 것이 좋습니다.
//
final int baseValue = 10;
Stream<Integer> stream = numbers.stream().map(n -> n + baseValue);
//