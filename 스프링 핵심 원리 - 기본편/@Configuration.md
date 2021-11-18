- Configuration 을 붙이면 **바이트코드를 조작하는 CGLIB 기술** 을 사용해서 싱글톤을 보장. 

- Configuration 을 붙이지 않고 @Bean만 적용하면 싱글톤이 보장 되지 않는다.
(@Bean만 사용해도 스프링 빈으로 등록은 됨.)

- 스프링 설정 정보는 항상 @Configuration을 사용!