## equals()와 hashcode()에 대해 설명해 주세요.

자바의 모든 클래스가 상속 받는 Object 클래스에 속한 메서드입니다.
equals() 메서드는 두 객체의 동등성을 비교하는 데 사용됩니다. hashCode 메서드는
객체의 해시코드 값을 반환하는 데 사용됩니다. 

equals() 메서드를 재정의하지 않으면 객체의 동등성 비교는 객체의 참조를 비교하는 것으로 제한됩니다. 따라서 값으로 두 객체를 논리적으로 같게 간주하고자 한다면 equals() 메서드를 오버라이딩 해야 합니다. hashCode() 메서드는 해시 기반의 컬렉션에 저장할 때 사용하는 해시 코드를 제공합니다. equals()의 결과가 true인 두 객체의 해시코드는 반드시 같아야 한다는 자바의 규칙 때문 hashCode() 도 같이 재정의해줘야 합니다. 두 메서드를 함께 재정의하지 않을 경우, 해시 기반 컬렉션에서 객체가 올바르게 작동하지 않을 수도 있습니다. 


즉, 항상 오버라이딩 할 필요는 없지만, 객체의 논리적 동등성을 비교하거나 해시 기반 컬렉션에서 사용될 경우 반드시 둘 다 오버라이딩 해야 합니다.

<br/>

__equals() 와 hashCode() 오버라이딩__
```java
public class Person {
    private String id;
    private String name;

    public Person(String id, String name) {
        this.id = id;
        this.name = name;
    }

    // id와 name 필드가 같으면 두 객체가 동일하다고 간주한다. 
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return id.equals(person.id) && name.equals(person.name);
    }

    // id와 name을 기반으로 hashCode를 생성한다. 
    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }

    // getters and setters
}
```

<br/>

> __HashMap의 동작 원리__: 
>1. 해시 코드 생성(hashCode() 메소드 호출) 
> 2. 해시 코드에 따른 버킷(배열의 인덱스와 같은 느낌) 결정 
>3. 버킷에 객체 저장 
>4. 객체 검색 : 검색할 객체의 해시 코드 생성, 해시 코드에 해당하는 버킷을 찾아, 그 버킷 내에서 equals() 메소드를 사용하여 실제 객체를 찾는다.  

![image](https://github.com/chunghye98/KB_CS_Study/assets/57451700/7ef65dc6-ba8c-4bd9-9b91-ad5db7f32212)

<br/>

> __동일성__: 두 객체가 동일한 메모리 주소를 가리키는지를 의미한다. 즉, 두 객체가 같은 인스턴스인지 확인한다. 자바에서는 `==` 연산자를 사용하여 동일성을 비교한다. 

> __동등성__: 두 객체가 논리적으로 같은지를 의미한다. 즉, 객체의 상태나 값이 같은지를 확인한다. 자바에서는 `equals()` 메소드를 오버라이딩하여 객체의 동등성을 정의한다. 


<hr/>


### 본인이 hashcode() 를 정의해야 한다면, 어떤 점을 염두에 두고 구현할 것 같으세요?

먼저 equals() 메서드를 재정의할 것 같습니다. 

<hr/>


### 그렇다면 equals() 를 재정의 해야 할 때, 어떤 점을 염두에 두어야 하는지 설명해 주세요.
객체의 값들이 논리적으로 동등한지를 알고 싶어서 재정의하는 것임을 생각해야 합니다. 

<hr/>

### 참고
[자바의 Object 클래스와 equals, hashCode 메서드의 중요성](https://f-lab.kr/insight/java-object-class-equals-hashcode?gad_source=1&gclid=Cj0KCQjw4MSzBhC8ARIsAPFOuyVMJOSlO4-yNxA2CcqBvh6i7yymsQ-Lh1N7ikkeYi4kSIo_UPPCb0YaAkxREALw_wcB)