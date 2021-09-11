## Optional

### Optional 소개

자바 프로그래밍에서 NullPointerException을 종종 보게 되는 이유

- null을 리턴하니깐! && null 체크를 깜빡했으니까!

#### Java 8 버전 이전

메소드에서 작업 중 특별한 상황에서 값을 제대로 리턴할 수 없는 경우 선택할 수 있는 방법.

- 예외를 던진다(비싸다, 스택트레이스를 찍어두기 때문에 리소스가 발생)

- null을 리턴한다(비용 문제가 없지만 그 코드를 사용하는 클라이언트 코드가 주의해야 한다)

#### Java 8 부터

- Optional을 리턴한다. (클라이언트에 코드에게 명시적으로 빈 값일 수도 있다는 걸 알려주고, 빈 값인 경우에 대한 처리를 강제한다.)

- 오직 값 한 개가 들어있을 수도 없을 수도 있는 컨테이너.

### 주의

- 리턴값으로만 쓰기를 권장한다. (메소드 매개변수 타입, 맵의 키 타입, 인스턴스 필드 타입으로 쓰지 말자.)

- Optional을 리턴하는 메소드에서 null을 리턴하지 말자.

- 프리미티브 타입용 Optional은 따로 있다. OptionalInt, OptionalLong

- Collection, Map, Stream, Array, Optional(그 자체 만으로도 값이 null인지 판단할 수 있는 컨테이너 성격의 인스턴스)은 Optional로 감싸지 말 것.

----

### Optional Api

#### Optional 만들기

- Optional.of() 

    : Optional 안에 들어오는 객체가 null이 아니다. 즉 null이 들어오게 되면 NPE 발생.

- Optional.ofNullable()

    : Optional 안에 들어오는 객체가 null일 수도 있다.
    
- Optional.Empty()

    : Optional 안에 빈 객체(Null)를 담아서 반환.
    
#### Optional에 값이 있는지 없는지 확인하기

- isPresent() 

    : 옵셔널 안에 값이 있으면 true
    
- isEmpty()

    : 옵셔널 안에 값이 없으면 false
    
#### Optional에 있는 값 가져오기

- get()

- 만약에 비어있는 Optional에 무엇가를 꺼낸다면 NoSuchElementException 발생.

#### Optional에 값이 있는 경우에 그 값을 가지고 ~~~를 하라

- ifPresent(Consumer)

- 예) Spring으로 시작하는 수업이 있으면 id를 출력하라

#### Optional에 값이 있으면 가져오고 없는 경우에 ~~~를 리턴하라

- orElse(T)

- 메서드 레퍼런스,람다, 함수형 인터페이스를 주는 것이 아닌 OnlineClass 타입을 넘겨줌, Optional 안에 값이 있든 없든 orElse 구문 실행.

- 예) JPA로 시작하는 수업이 없다면 비어있는 수업을 리턴하라.

#### Optional에 값이 있으면 가져오고 없는 경우 ~~를 하라.

- orElseGet(Supplier)

- 메서드 레퍼런스, 람다, 함수형 인터페이스를 넘겨줌, Optional 안에 값이 없을 경우에만 orElseGet 실행.

- 예) JPA로 시작하는 수업이 없다면 새로 만들어서 리턴하라.

#### Optional에 값이 있으면 가져오고 없는 경우 에러를 던져라.

- orElseThrow()

#### Optional에 들어있는 값 걸러내기.

- Optional filter(Predicate)

#### Optional에 들어있는 값 변환하기

- Optional map(Function)

- Optional flatMap(Function) : Optional 안에 들어있는 인스턴스가 Optional인 경우에 사용하면 편리.

