## OSIV와 성능 최적화

- Open Session in View : 하이버네이트

- Open EntityManager in View : Jpa

(관례상 OSIV라 한다.)

### OSIV ON

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/816e3b74-6375-41a6-8800-921faaa5e478/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.03.01.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211103T131459Z&X-Amz-Expires=86400&X-Amz-Signature=1e919e5cfd7ead639d64d2bcbb3327661d40fdd935a8592b767ecaf6fd512091&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-11-03%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%252010.03.01.png%22)

spring.jpa.open-in-view : true 기본 값

이 기본값을 뿌리면서 애플리케이션 시작 시점에 warn 로그를 남기는 것은 이유가 있다.

OSIV 전략은 트랜잭션 시작처럼 최초 데이터베이스 커넥션 시작 시점부터 API 응답이 끝날 때 까지 영속성 컨텍스트와 데이터베이스 커넥션을 유지한다.
그래서 지금까지 View Template이나 API 컨트롤러에서 **지연 로딩**이 가능했던 것이다.

지연 로딩은 영속성 컨텍스트가 살아있어야 가능하고, 영속성 컨텍스트는 기본적으로 데이터베이스 커넥션을 유지한다. 이것 자체가 큰 장점이다

하지만,,,

이 전략은 너무 오랜시간동안 데이터베이스 커넥션 리소스를 사용하기 때문에, 실시간 트래픽이 중요한 애플리케이션에서는 커넥션이 모자랄 수 있다. 이것은 결국 장애로 이어진다.
예를 들어서 컨트롤러에서 외부 API를 호출하면 외부 API 대기 시간 만큼 커넥션 리소스를 반환하지 못하고, 유지해야 한다.

### OSIV OFF 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0e572d17-eeed-4b42-b5dc-13ee79909306/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.19.14.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211103T131955Z&X-Amz-Expires=86400&X-Amz-Signature=b6587348733a6c213a3913ffa423846b3cea7d5424dbd99a751993bfca77e108&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-11-03%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%252010.19.14.png%22)

spring.jpa.open-in-view : false OSIV 종료

OSIV를 끄면 트랜잭션을 종료할 때 영속성 컨텍스트를 닫고, 데이터베이스 커넥션도 반환한다. 따라서 커넥션 리소르를 낭비하지 않는다.

OSIV를 끄면 모든 지연로딩을 트랜잭션 안에서 처리해야 한다. 따라서 지금까지 작성한 많은 지연로딩 코드를 트랜잭션 안으로 넣어야 하는 단점이 있다.
그리고 view template에서 지연 로딩을 강제로 호출해야 한다.

## 커맨드와 쿼리 분리
실무에서 OSIV를 끈 상태로 복잡성을 관리하는 좋은 방법이 있다. 바로 Command와 Query를 분리하는 것이다.

보통 비즈니스 로직은 특정 엔티티 몇개를 등록하거나 수정하는 것이므로 성능이 크게 문제가 되지 않는다. 그런데 복잡한 화면을 출력하기 위한 쿼리는 화면에 맞추어 성능을 최적화 하는 것이 중요하다.
하지만 그 복잡성에 비해 핵심 비즈니스에 큰 영향을 주는것은 아니다.

그래서 크고 복잡한 애플리케이션을 개발한다면, 이 둘의 관심사를 명확하게 분리하는 선택은 유지보수 관점에서 충분히 의미가 있다.

단순하게 설명해서 다음 처럼 분리하는 것이다.

- OrderService

    - OrderService : 핵심 비즈니스 로직
    
    - OrderQueryService : 화면이나 API에 맞춘 서비스(주로 읽기 전용 트랜잭션 사용)
    
보통 서비스 계층에서 트랜잭션을 유지한다. 두  서비스 모두 트랜잭션을 유지하면서 지연 로딩을 사용할 수 있다.
 
예) 고객 서비스의 실시간 API는 OSIV를 끄고, ADMIN 처럼 커넥션을 많이 사용하지 않는 곳에서는 OSIV를 켠다.