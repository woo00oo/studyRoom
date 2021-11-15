## BeanFactory

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0061422b-cd91-4139-be35-04c2cedbce99/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.12.06.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211115T132356Z&X-Amz-Expires=86400&X-Amz-Signature=f928269e6379c6799cab67696e7541dd986220e4ad15ce579022ea108c88bfc3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-11-15%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%252010.12.06.png%22)

- 스프링 컨테이너의 최상위 인터페이스.

- 스프링 빈을 관리하고 조회하는 역할을 담당.

- getBean()을 제공

## ApplicationContext 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1a2dcbde-6b5d-495d-8141-ccf11b9b2475/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.12.42.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211115T132351Z&X-Amz-Expires=86400&X-Amz-Signature=7406a8300471acac9d5a5a0649648c45ae495d32c28c3dad208e4b13d966c81b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-11-15%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%252010.12.42.png%22)

- BeanFactory 기능을 모두 상속받아서 제공.

- 빈을 관리하고 조회하는 기능 + 수 많은 부가 기능 제공.

- 메시지 소스를 활용한 국제화 기능

    - 예를 들어서 한국에서 들어오면 한국어로, 영어권에서 들어오면 영어로 출력
    
- 환경 변수

    - 로컬, 개발, 운영등을 구분해서 처리
    
- 애플리케이션 이벤트

    - 이벤트를 발행하고 구독하는 모델을 편리하게 지원
    
- 편리한 리소스 조회

    - 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회

> BeanFactory 나 ApplicationContext를 스프링 컨테이너라고 한다.