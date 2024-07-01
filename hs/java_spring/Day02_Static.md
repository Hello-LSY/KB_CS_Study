## static class와 static method를 비교해 주세요.

static class 는 인스턴스화 될 수 없는 클래스를 의미합니다. 주로 정적 메소드, 정적 속성을 포함할 때 사용합니다. 예를 들어, 유틸리티 함수나 상수를 모아둘 때 사용합니다. 인스턴스화가 불가능하고, 캡슐화를 하기 어려워 객체지향프로그래밍을 추구하는 Java에서는 이 static class를 사용하지 못하게 만들었습니다만, inner static class를 사용할 수 있습니다. 예를 들어, Java의 Collections 클래스가 있습니다.
```java
public class Collections {
    // Suppresses default constructor, ensuring non-instantiability.
    private Collections() {
    }
    static class UnmodifiableCollection<E> implements Collection<E>, Serializable {
        private static final long serialVersionUID = 1820017752578914078L;

        final Collection<? extends E> c;

        UnmodifiableCollection(Collection<? extends E> c) {
            if (c==null)
                throw new NullPointerException();
            this.c = c;
        }

        public int size()                          {return c.size();}
        public boolean isEmpty()                   {return c.isEmpty();}
        public boolean contains(Object o)          {return c.contains(o);}
        public Object[] toArray()                  {return c.toArray();}
        public <T> T[] toArray(T[] a)              {return c.toArray(a);}
        public <T> T[] toArray(IntFunction<T[]> f) {return c.toArray(f);}
        public String toString()                   {return c.toString();}

        public Iterator<E> iterator() {
            return new Iterator<E>() {
                private final Iterator<? extends E> i = c.iterator();

                public boolean hasNext() {return i.hasNext();}
                public E next()          {return i.next();}
                public void remove() {
                    throw new UnsupportedOperationException();
                }
                @Override
                public void forEachRemaining(Consumer<? super E> action) {
                    // Use backing collection version
                    i.forEachRemaining(action);
                }
            };
        }

        public boolean add(E e) {
            throw new UnsupportedOperationException();
        }
        public boolean remove(Object o) {
            throw new UnsupportedOperationException();
        }

        public boolean containsAll(Collection<?> coll) {
            return c.containsAll(coll);
        }
        public boolean addAll(Collection<? extends E> coll) {
            throw new UnsupportedOperationException();
        }
        public boolean removeAll(Collection<?> coll) {
            throw new UnsupportedOperationException();
        }
        public boolean retainAll(Collection<?> coll) {
            throw new UnsupportedOperationException();
        }
        public void clear() {
            throw new UnsupportedOperationException();
        }

        // Override default methods in Collection
        @Override
        public void forEach(Consumer<? super E> action) {
            c.forEach(action);
        }
        @Override
        public boolean removeIf(Predicate<? super E> filter) {
            throw new UnsupportedOperationException();
        }
        @SuppressWarnings("unchecked")
        @Override
        public Spliterator<E> spliterator() {
            return (Spliterator<E>)c.spliterator();
        }
        @SuppressWarnings("unchecked")
        @Override
        public Stream<E> stream() {
            return (Stream<E>)c.stream();
        }
        @SuppressWarnings("unchecked")
        @Override
        public Stream<E> parallelStream() {
            return (Stream<E>)c.parallelStream();
        }
    }
}
```

static method는 클래스의 인스턴스에 속하지 않는 메소드입니다. 클래스 자체에서 호출할 수 있으며, 인스턴스화 없이 사용할 수 있습니다. 프로그램 실행 시 JVM의 runtime data area에 로드 시 method area에 올라가기 때문에 모든 쓰레드가 공유 가능합니다. 즉, 미리 올라가있고 GC가 삭제하지 않기 때문에 접근이 빠릅니다.
예를 들어, Java의 Math 클래스가 있습니다.

__Java의 Math 클래스__
```java
public final class Math {

    private Math() {}

    public static final double E = 2.7182818284590452354;
    public static final double PI = 3.14159265358979323846;
    private static final double DEGREES_TO_RADIANS = 0.017453292519943295;
    private static final double RADIANS_TO_DEGREES = 57.29577951308232;

    @HotSpotIntrinsicCandidate
    public static double sin(double a) {
        return StrictMath.sin(a); 
    }
    ...
}
```

### static 을 사용하면 어떤 이점을 얻을 수 있나요? 어떤 제약이 걸릴까요?
__static__ 키워드를 통해 Static 영역에 할당된 메모리는 JVM에서 static 영역에서 관리됩니다. 모든 객체가 공유하는 메모리라는 장점을 가지지만, GC의 관리 영역 밖이기 때문에 프로그램 종료 시까지 메모리가 할당되기 때문에 많이 사용할 경우 시스템의 퍼포먼스에 영향을 줄 수 있습니다.

### 컴파일 과정에서 static 이 어떻게 처리되는지 설명해 주세요.
컴파일 과정에서 
