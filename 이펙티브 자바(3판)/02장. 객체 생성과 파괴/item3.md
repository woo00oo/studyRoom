# item3 : private 생성자나 열거 타입으로 싱글턴임을 보증하라.

> 싱글턴(singleton)이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다.

싱글턴의 전형적인 예로는 함수와 같은 무상태 객체나 설계상 유일해야 하는 시스템 컴포넌트를 들 수 있다.

#### 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있다.

> 타입을 인터페이스로 정의한 다음 그 인터페이스를 구현해서 만든 싱글턴이 아니라면 싱글턴 인스턴스를 가짜(mock) 구현으로 대체할 수 없기 때문.


### 싱글 턴을 만드는 방식

#### 1. public static 멤버를 하나 두는 것.

```java

public class Elvis {

    public static final Elvis INSTANCE = new Elvis();

    private Elvis(){...}

    public void leaveTheBuilding(){...}

```

- private 생성자는 public static final 필드인 Elvis.INSTANCE를 초기화할 때 딱 한 번만 호출.

- public이나 protected 생성자가 없으므로 Elvis 클래스가 초기화될 때 만들어진 인스턴스가 전체 시스템에서 하나뿐임이 보장.

- 해당 클래스가 싱글턴임이 API에 명백히 드러난다는 것.

- public static 필드가 final이니 절대로 다른 객체를 참조할 수 없다.

- 간결함

#### 정적 팩터리 메서드

```java

public class Elvis{

    private static final Elvis INSTANCE = new Elvis();

    private Elvis(){...}

    public static Elvis getInstance(){ return INSTANCE };

    public void leaveTheBuilding(){...}


```

- (마음이 바뀌면) API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있음.
 
    유일한 인스턴스를 반환하던 팩터리 메서드가 (예컨대) 호출하는 쓰레드별로 다른 인스턴스를 넘겨주게 할 수 있다.
    
- 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있음.

- 정적 팩터리의 메서드 참조를 공급자(supplier)로 사용할 수 있다는 점.

    ex) Elvis::get Instance를 Supplier<Elvis>로 사용.

- 이러한 장점들이 굳이 필요하지 않다면 public 필드 방식이 좋음.


둘 중 하나의 방식으로 만든 싱글턴 클래스를 직렬화하려면 단순히 Serializable을 구현한다고 선언하는 것만으로 부족.

모든 인스턴스 필드를 일시적(transient)이라고 선언하고 readResolve 메서드를 제공해야 한다. 이렇게 하지 않으면 직렬화된 인스턴스를 역직렬화할 때 마다 새로운 인스턴스가 만들어진다.

```java

// 싱글턴임을 보장해주는 readResolve 메서드
public Object readResolve(){

    //'진짜' Elvis를 반환하고, 가짜 Elvis는 가비지 컬렉터에 맡긴다.
    return INSTANCE;

}

```

#### enum 타입

```java

public enum Elvis {

    INSTANCE;

    public void leaveTheBuilding() {...}

}

```

- public 필드 방식과 비슷하지만, 더 간결하고, 추가 노력 없이 직렬화할 수 있고, 심지어 아주 복잡한 직렬화 상황이나 리플렉션 공격에서도 제2의 인스턴스가 생기는 일을 완벽히 막아준다.

- 대부분 상황에서는 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법.

- 만들려는 싱글턴이 Enum 외의 클래스를 상속해야 한다면 이 방법은 사용할 수 없다.