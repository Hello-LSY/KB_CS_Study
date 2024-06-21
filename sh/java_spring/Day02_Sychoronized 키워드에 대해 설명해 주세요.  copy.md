7. synchronized 키워드에 대해 설명해 주세요
synchronized 키워드는 Java에서 여러 스레드가 동시에 접근할 수 있는 자원에 대해 동기화를 제공하여 데이터 무결성을 보장합니다.

7-1. synchronized 키워드가 어디에 붙는지에 따라 의미가 약간씩 변화하는데, 각각 어떤 의미를 갖게 되는지 설명해주세요
-인스턴스 메서드에 synchronized: 인스턴스 메서드에 synchronized 키워드를 붙이면 해당 메서드를 호출하는 모든 스레드가 메서드 실행 전에 해당 객체의 잠금을 획득해야 합니다.
//
public synchronized void syncMethod() {
    // 동기화된 코드
}
//
-정적 메서드에 synchronized: 정적 메서드에 synchronized 키워드를 붙이면 해당 메서드를 호출하는 모든 스레드가 메서드 실행 전에 해당 클래스의 잠금을 획득해야 합니다.
//
public static synchronized void syncStaticMethod() {
    // 동기화된 코드
}
//
-블록에 synchronized: 특정 블록을 동기화하려면 synchronized 블록을 사용하여 특정 객체의 잠금을 획득한 후 블록 내부의 코드를 실행합니다.
//
public void syncBlock() {
    synchronized (this) {
        // 동기화된 코드
    }
}
//

7-2. 효율적인 코드 작성 측면에서, synchronized는 좋은 키워드일까요?
synchronized 키워드는 간단하고 강력한 동기화 메커니즘을 제공하지만, 과도하게 사용하면 성능 저하와 교착 상태(deadlock)를 유발할 수 있습니다. 효율적인 동기화를 위해서는 최소한의 코드 블록에만 synchronized를 적용하고, 필요한 경우 java.util.concurrent 패키지의 동기화 도구를 사용하는 것이 좋습니다.

7-3. synchronized를 대체할 수 있는 자바의 다른 동기화 기법에 대해 설명해주세요
-ReentrantLock: java.util.concurrent.locks 패키지에 있는 ReentrantLock 클래스는 더 세밀한 동기화 제어를 제공합니다.
//
Lock lock = new ReentrantLock();
lock.lock();
try {
    // 동기화된 코드
} finally {
    lock.unlock();
}
//
-Concurrent Collections: java.util.concurrent 패키지에 있는 동기화된 컬렉션을 사용합니다. 예를 들어, ConcurrentHashMap, `

CopyOnWriteArrayList` 등이 있습니다. 이러한 컬렉션은 내부적으로 동기화 메커니즘을 제공하여 스레드 안전성을 보장합니다.

-Atomic Variables: java.util.concurrent.atomic 패키지에 있는 원자적 변수를 사용하여 동기화 없이도 스레드 안전한 연산을 수행할 수 있습니다. 예를 들어, AtomicInteger, AtomicLong 등이 있습니다.
//
AtomicInteger atomicInteger = new AtomicInteger(0);
atomicInteger.incrementAndGet();
//

7-4. Thread Local에 대해 설명해 주세요
ThreadLocal은 각 스레드마다 독립적인 변수를 제공하는 클래스입니다. 각 스레드가 ThreadLocal 객체에 접근할 때마다, 해당 스레드에 대한 고유한 값을 반환하므로 스레드 간의 공유 상태를 방지할 수 있습니다. 이를 통해 스레드 안전성을 확보할 수 있습니다.
//
public class ThreadLocalExample {
    private static final ThreadLocal<Integer> threadLocalValue = ThreadLocal.withInitial(() -> 1);

    public int getValue() {
        return threadLocalValue.get();
    }

    public void setValue(int value) {
        threadLocalValue.set(value);
    }
}
//