## Java의 GC에 대해 설명해 주세요.

__개념__<br/>
Garbage Collection(GC)는 프로그래밍 언어에서 더 이상 사용되지 않는 객체를 자동으로 메모리에서 해제하는 메모리 관리 기법입니다.

__목적__<br/>
메모리 누수를 방지하고, 개발자가 수동으로 메모리를 관리하는 부담을 줄이는 것이 GC의 목적입니다.

__GC의 대상 영역__<br/>
JVM의 Runtime Data Area에서 Heap 메모리 영역은 다음 그림과 같이 쪼갭니다.

<img src="https://velog.velcdn.com/images%2Frecordsbeat%2Fpost%2F682408fc-f29e-42e9-b980-3d6f1d6c4989%2Fimage.png" width="50%" />


**Young** : 비교적 최근에 참조가 일어난 곳
- eden : young 영역 중에서도 특히 방금 막 생성된 것들이 위치한다
- survivor : eden에서 생존한 것들이 당분간 위치하는 곳

**Old**: 특정 횟수 이상을 살아남은 참조가 살아있는 곳

**Permanent**: Method Area의 메타 정보가 기록된 곳 

__동작 방식__<br/>
1. Stop the World<br/>
GC를 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업입니다. GC가 실행될 때는 GC를 실행하는 스레드를 제외한 모드 스레드들의 작업이 중단되고, GC가 완료되면 작업이 재개됩니다.

2. Mark and Sweep<br/>
`Mark` : 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업    
`Sweep` : Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업    

Stop the World를 하고 GC는 스택의 변수 또는 객체를 스캔하면서 각각이 어떤 객체를 참조하고 있는지 탐색합니다. 사용되고 있는 메모리를 식별(Mark)하고, Mark되지 않은 객체들을 메모리에서 제거(Sweep)합니다. 

__GC 종류__
1. Minor GC<br/>
    Young 영역에 대한 GC, Eden 영역이 다 차면 발생합니다. 살아남은 객체는 Survivor 영역으로 옮겨집니다.    
   한 개의 Survivor 영역이 다 차면 다른 Survivor 영역으로 옮겨 1개의 Survivor 영역은 반드시 빈 상태가 되게 만듭니다. 
2. Major GC<br/>
    Old 영역에 대한 GC, Young 영역에서 오래 살아남은 객체가 Old 영역으로 이동하여 Old 영역의 메모리가 부족해지면 Major GC가 발생합니다.

__GC 알고리즘__    
1. Serial GC<br/>
싱글스레드 애플리케이션을 위한 GC, Young 영역에서는 Mark Sweep 알고리즘을 수행한다. Old 영역에서는 Mark Sweep 알고리즘에 Compact 작업을 추가합니다. Compact란 Heap 영역을 정리하기 위한 단계로 유효한 객체를 한 곳으로 몰고 객체가 존재하지 않는 부분으로 만드는 방식입니다.

2. Parallel GC<br/>
멀티스레드 애플리케이션을 위한 GC, 여러 개의 스레드를 통해 Parallel 하게 GC를 수행함으로써 GC의 오버헤드를 상당히 줄여줍니다. 그러나, 애플리케이션을 멈추는 것을 피하진 못합니다.

3. CMS(Concurrent Mark Sweep) GC<br/>
Parallel GC와 마찬가지로 여러 개의 스래드를 이용합니다. 다른 GC와는 다르게 Mark Sweep 알고리즘을 Concurrent하게 수행합니다. 애플리케이션의 지연 시간을 최소화하여 애플리케이션이 구종 중일 때 멈추지 않고 GC를 이용 가능합니다. 그러나 다른 GC 방식보다 하드웨어 자원을 더 많이 필요로 하며 compact 단계를 수행하지 않아 메모리 파편화가 심해질 수도 있습니다. 만약 이 때문에 메모리 공간이 부족해지면 Serial GC와 똑같이 동작합니다.

4. G1(Garbage First) GC<br/>
장기적으로 문제될 수 있는 CMS GC를 대체하기 위해 개발되었고, Java 7부터 지원합니다.
기존 GC 알고리즘 알고리즘은 Heap 영역을 Young과 Old 영역으로 나누었지만 G1 GC는 물리적으로 메모리 공간을 나누지 않습니다. 대신 Region 이라는 개념을 도입하여 Heap을 균등하게 여러 지역으로 나누고, 각 지역을 역할과 함께 논리적으로 구분(Eden, Survivor, Old)하여 객체를 할당합니다. 더해서 Humonogous와 Availab/e/Unused 라는 역할을 추가하여 Region 크기의 50%를 초과하면 Humonogous, 사용되지 않는 Region을 Available/Unused 로 지정합니다. 이후 Gargabe가 많은 Region에 대해 우선적으로 GC를 수행합니다. 

한 지역에서 Minor GC가 실행되면 Garbage가 가장 많은 지역을 찾아 Mark And Sweep을 합니다. Eden 지역에서 GC가 수행되면 살아남은 객체를 Available/Unused 지역을 옮기고 Survivor 로 변경합니다. Eden 영역은 Available/Unused 으로 변경됩니다.

시스템이 계속 운영되다가 객체가 너무 많아 메모리를 빠르게 회수할 수 없을 때 Major GC가 수행됩니다. G1 GC는 다른 알고리즘과 다르게 Garbage가 많은 영역에서만 GC를 Concurrent하게 수행되기 때문에 애플리케이션의 지연을 최소화 시킵니다. 

이러한 구조는 다른 방식보다 처리 속도가 빠르고 효율적이기 때문에 Java9부터 기본 GC로 사용되게 되었습니다. 


__장점__<br/>
1. 메모리 누수 방지
2. 코드 간결성
    메모리 코드를 작성하지 않아도 되므로, 코드가 간결해집니다.

__단점__<br/> 
1. 시스템 자원을 사용하기 때문에 성능 오버헤드가 발생할 수 있습니다.
2. GC가 언제 실행될지 예측하기 어려울 수 있습니다. 

### finalize() 를 수동으로 호출하는 것은 왜 문제가 될 수 있을까요?
Object 클래스의 finalize() 메서드는 호출되더라도 즉시 실행된다는 보장이 없으며, 언제 수행되는지도 알 수 없습니다.
즉, 예측 불가능하기 때문에 애플리케이션 실행 중에 리소스 문제가 발생할 수 있으며 디버깅하기도 쉽지 않습니다. 
 
finalize() 메서드 안에서 예외가 발생한다 해도 무시가 되며 finalize() 메서드도 종료됩니다. 

더해서, finalize()를 재정의하는 것만으로도 성능 저하가 발생합니다.

이와 같은 이유로 인해 finalize()를 수동으로 호출하는 것은 무제가 될 수 있습니다.

### 어떤 변수의 값이 null이 되었다면, 이 값은 GC가 될 가능성이 있을까요?
변수의 참조를 null로 변경한다면, 원래 참조하고 있던 메모리는 참조가 끊어지기 때문에 GC의 대상이 될 수 있습니다. 


### 참고
[[Java] Garbage Collection(가비지 컬렉션)의 개념 및 동작 원리 (1/2)
출처: https://mangkyu.tistory.com/118 [MangKyu's Diary:티스토리]](https://mangkyu.tistory.com/118

[[Java] 다양한 종류의 Garbage Collection(가비지 컬렉션) 알고리즘 (2/2)
출처: https://mangkyu.tistory.com/119 [MangKyu's Diary:티스토리]](https://mangkyu.tistory.com/119)

[Garbage Collector 제대로 알기](https://velog.io/@recordsbeat/Garbage-Collector-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B8%B0)    
