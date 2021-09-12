## Date와 Time API 소개

### 자바 8에 새로운 날짜와 시간 API가 생긴 이유

- 그전까지 사용하던 java.util.Date 클래스는 mutalbe(불변성) 하기 때문에 thread safe하지 않다.

- 클래스 이름이 명확하지 않다. Date인데 시작까지 다룬다.

- 버그 발생할 여지가 많다. (타입 안정성이 없고, 월이 0부터 시작한다거나.. 월에 음수가 들어갈 수 있음)

- 날짜 시간 처리가 복잡한 애플리케이션에서는 보통 Joda Time을 쓰곤 했다.

### 자바 8에서 제공하는 Date-time API

- JSR-310 스팩의 구현체를 제공한다.

- 디자인 철학

    - clear
    
    - Fluent
    
    - Immutable
    
    - Extensible
    
### 주요 API

- 기계용 시간(machine time)과 인류용 시간(human time)으로 나눌 수 있다.

- 기계용 시간은 EPOCK(1970년 1월 1일 0시 0분 0초)부터 현재까지의 타임스탬프를 표현한다.

- 인류용 시간은 우리가 흔히 사용하는 년, 월, 일, 시, 분, 초 등을 표현한다.

- 타임스탬프는 Instant를 사용한다.

- 특정 날짜(LocalDate), 시간(LocalTime), 일시(LocalDateTime)를 사용할 수 있다.

- 기간을 표현할 때는 Duration(시간 기반)과 Period(날짜 기반)를 사용할 수 있다.

- DateTimeFomatter를 사용해서 일시를 특정한 문자열로 포매팅할 수 있다.

---

## Date와 Time API

### 지금 이 순간을 기계 시간으로 표현 하는 방법

- Instant.now() : 현재 UTC(GMT)를 리턴한다.

- Universal Time Coordinated == Greenwich Mean Time


```java

Instant now = Instant.now(); 
System.out.println(now); 
System.out.println(now.atZone(ZoneId.of("UTC")));
ZonedDateTime zonedDateTime = now.atZone(ZoneId.systemDefault()); 
System.out.println(zonedDateTime);

```

### 인류용 일시를 표현하는 방법

- LocalDateTime.now() : 현재 시스템 Zone에 해당하는(로컬)일시를 리턴한다.

- LocalDateTime.of(int, Month, int, int, int, int) : 로컬의 특정 일시를 리턴한다.

- ZonedDateTime.of(int, Month, int, int, int, int, ZoneId) : 특정 Zone의 특정 일시를 리턴한다.

### 기간을 표현하는 방법

- Period / Duration . between()

```java

Period between = Period.between(today, birthDay); 
System.out.println(between.get(ChronoUnit.DAYS));

```

### 파싱 또는 포매팅

- 미리 정의해둔 포맷 참고할 수 있음

- LocalDateTime.parse(String, DateTimeFormatter);

- DateTimeFormatter

```java

DateTimeFormatter formatter = DateTimeFormatter.ofPattern("MM/d/yyyy");
LocalDate date = LocalDate.parse("07/15/1982", formatter); 
System.out.println(date); 
System.out.println(today.format(formatter));

```

### 레거시 API 지원

- GregorianCalendar와 Date 타입의 인스턴스를 Instant나 ZondedDateTime으로 변환 가능.

- java.util.TimeZone에서 java.time.ZoneId로 상호 변환 가능


```java

ZoneId newZoneAPI = TimeZone.getTimeZone("PST").toZoneId();
TimeZone legacyZoneAPI = TimeZone.getTimeZone(newZoneAPI);

Instant newInstant = new Date().toInstant(); 
Date legacyInstant = Date.from(newInstant);

```