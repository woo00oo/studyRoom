## Stream 소개

### Stream

- sequence of elements supporting sequential and parallel aggregate opertaions

- 데이터를 담고 있는 저장소 (컬렉션)이 아니다.

- Funtional in nature, 스트림이 처리하는 데이터 소스를 변경하지 않는다.

- 스트림으로 처리하는 데이터는 오직 한번만 처리한다.

- 무제한일 수도 있다.(Short Circuit 메소드를 사용해서 제한할 수 도 있다.)

- 중개 오퍼레이션은 근본적으로 lazy하다.

- 손쉽게 병렬 처리할 수 있다.

### 스트림 파이프라인

- 0 또는 다수의 중개 오퍼레이션과 한개의 종료 오퍼레이션으로 구성한다.

- 스트림의 데이터 소스는 오직 터미널 오퍼레이션을 실행할 때에만 처리한다.

### 중개 오퍼레이션

- 스트림을 리턴한다.

- Stateless(무상태성) / Stateful(상태성) 오퍼레이션으로 더 상세하게 구분할 수 도 있다.(대부분은 Stateless지만 distinct나 sort
처럼 이전 소스 데이터를 참조해야 하는 오퍼레이션은 Stateful 오퍼레이션이다.)

- filter, map, limit, skip, sorted, 

### 종료(터미널) 오퍼레이션

- 스트림을 리턴하지 않는다.

- collect, allMatch, count, forEach, forEach, min, max


---

## Stream API

### 걸러내기

- Filter(Predicate)

- ex) 이름이 3글자 이상인 데이터만 새로운 스트림으로

### 변경하기 

- Map(Funtion) 또는 FlatMap(Function)

- ex) 각각의 Post 인스턴스에서 String title만 새로운 스트림으로

- ex) List<Stream<String>>을 String으로

### 생성하기

- generate(Supplier) 또는 Iterate(T seed, UnaryOperator)

- ex) 10부터 1씩 증가하는 무제한 숫자 스트림

- ex) 랜덤 int 무제한 스트림

### 제한하기

- limit(long) 또는 skip(long)

- ex) 최대 5개의 요소가 담긴 스트림을 리턴한다.

- ex) 앞에서 3개를 뺀 나머지 스트림을 리턴한다.

### 스트림에 있는 데이터가 특정 조건을 만족하는지 확인

- anyMatch(), allMatch(), nonMatch()

- ex) k로 시작하는 문자열이 있는지 확인한다. (true 또는 false를 리턴한다.)

- ex) 스트림에 있는 모든 값이 10보다 작은지 확인한다.

### 개수 세기

- count()

- ex) 10보다 큰 수의 개수를 센다.

### 스트림을 데이터 하나로 뭉치기

- reduce(identity, BiFunction(), collect(), sum(), max())

- ex) 모든 숫자 합 구하기

- ex) 모든 데이터를 하나의 List 또는 Set에 옮겨 담기


## Stream 예제

```java

        List<OnlineClass> springClasses = new ArrayList<>();
        springClasses.add(new OnlineClass(1, "spring boot", true));
        springClasses.add(new OnlineClass(2, "spring data jpa", true));
        springClasses.add(new OnlineClass(3, "spring mvc", false));
        springClasses.add(new OnlineClass(4, "spring core", false));
        springClasses.add(new OnlineClass(5, "rest api development", false));

        List<OnlineClass> javaClasses = new ArrayList<>();
        javaClasses.add(new OnlineClass(6, "The Java, Test", true));
        javaClasses.add(new OnlineClass(7, "The Java, Code manipulation", true));
        javaClasses.add(new OnlineClass(8, "The Java, 8 to 11", false));

        List<List<OnlineClass>> keesunEvents = new ArrayList<>();
        keesunEvents.add(springClasses);
        keesunEvents.add(javaClasses);


        System.out.println("spring 으로 시작하는 수업");
        springClasses.stream()
                .filter(oc -> oc.getTitle().startsWith("spring"))
                .forEach(oc -> System.out.println(oc.getId()));

        System.out.println("close 되지 않은 수업");
        springClasses.stream()
                //.filter(oc -> !oc.isClosed())
                .filter(Predicate.not(OnlineClass::isClosed))
                .forEach(oc -> System.out.println(oc.getId()));


        System.out.println("수업 이름만 모아서 스트림 만들기");
        springClasses.stream()
                .map(OnlineClass::getTitle)
                .forEach(System.out::println);


        System.out.println("두 수업 목록에 들어있는 모든 수업 아이디 출력");
        keesunEvents.stream()
                .flatMap(Collection::stream)
                .forEach(oc -> System.out.println(oc.getId()));



        System.out.println("10부터 1씩 증가하는 무제한 스트림 중에서 앞에 10개 빼고 최대 10개 까지만");
        Stream.iterate(10, i -> i + 1)
                .skip(10)
                .limit(10)
                .forEach(System.out::println);


        System.out.println("자바 수업 중에 Test가 들어있는 수업이 있는지 확인");
        boolean test = javaClasses.stream().anyMatch(oc -> oc.getTitle().contains("Test"));
        System.out.println(test);


        System.out.println("스프링 수업 중에 제목에 spring이 들어간 제목만 모아서 List로 만들기");
        List<String> spring = springClasses.stream()
                .filter(oc -> oc.getTitle().contains("spring"))
                .map(OnlineClass::getTitle)
                .collect(Collectors.toList());
        spring.forEach(System.out::println);




```

