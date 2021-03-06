자동 프록시 생성기(AnnotationAwareAspectJAutoProxyCreator)는 Advisor를 자동으로 찾아와서 필요한 곳에 프록시를 생성하고 적용시켜 준다.
자동 프록시 생성기는 여기에 추가로 하나의 역할을 더 하는데, 바로 @Aspect를 찾아서 이것을 Advisor로 만들어준다.
쉽게 이야기해서 지금까지 학슴한 기능에 @Aspect를 Advisor로 변환해서 저장하는 기능도 한다.
그래서 이름 앞에 AnnotationAware(어노테이션을 인식하는)가 붙어 있는 것이다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3aba41fc-37a3-4fd7-9024-ac2699245e8f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-12-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.05.51.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211226%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211226T141921Z&X-Amz-Expires=86400&X-Amz-Signature=51e4805299819ae51dbbfac7849d231e847eed74e517bc3f11f1d59d4cc9216f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-12-26%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%252011.05.51.png%22&x-id=GetObject)

#### 자동 프록시 생성기는 2가지 일을 한다.

1. @Aspect를 보고 어드바이저(Advisor)로 변환해서 저장한다.

2. 어드바이저를 기반으로 프록시를 생성한다.

### 1. @Aspect를 어드바이저로 변환해서 저장하는 과정

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c9b4d1f0-72ab-4bbc-9f78-3cd2b90efda3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-12-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.06.59.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211226%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211226T141944Z&X-Amz-Expires=86400&X-Amz-Signature=69216886d27dbd8d7ab1aef5b17279a834a2793f73274620114af53eac9c6acd&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-12-26%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%252011.06.59.png%22&x-id=GetObject)

#### @Aspect를 어드바이저로 변환해서 저장하는 과정을 알아보자

1. 실행 : 스프링 애플리케이션 로딩 시점에 자동 프록시 생성기를 호출한다.

2. 모든 @Aspect 빈 조회 : 자동 프록시 생성기는 스프링 컨테이너에서 @Aspect 어노테이션이 붙은 스프링 빈을 모두 조회한다.

3. 어드바이저 생성 : @Aspect 어드바이저 빌더를 통해 @Aspect 어노테이션 정보를 기반으로 어드바이저를 생성한다.

4. @Aspect 기반 어드바이저 저장 : 생성한 어드바이저를 @Aspect 어드바이저 빌더 내부에 저장한다.

### @Aspect 어드바이저 빌더

BeanFactoryAspectJAdvisorBuilder 클래스이다. @Aspect의 정보를 기반으로 포인트 컷,
어드바이스, 어드바이저를 생성하고 보관하는 것을 담당한다. @Aspect의 정보를 기반으로 어드바이저를 만들고,
@Aspect 어드바이저 빌더 내부 저장소에 캐시한다. 캐시에 어드바이저가 이미 만들어져 있는 경우 캐시에 저장된 어드바이저를 반환한다.

### 2. 어드바이저를 기반으로 프록시 생성

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/eb36a8f3-1fa6-4544-ae52-056e84b11551/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-12-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.12.53.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211226%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211226T141954Z&X-Amz-Expires=86400&X-Amz-Signature=d7d8bcd932faee2b84fe459951a86e5dbb50ce146dfad81aed0f78c8022372ed&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-12-26%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%252011.12.53.png%22&x-id=GetObject)

자동 프록시 생성기의 작동 과정

1. 생성 : 스프링 빈 대상이 되는 객체를 생성한다. (@Bean, 컴포넌트 스캔 모두 포함)

2. 전달 : 생성된 객체를 빈 저장소에 등록하기 직전에 빈 후처리기에 전달한다.

3-1. Advisor 빈 조회 : 스프링 컨테이너에서 Advisor 빈을 모두 전달한다.

3-2. @Aspect Advisor 조회 : @Aspect 어드바이저 빌더 내부에 저장된 Advisor를 모두 조회한다.

4. 프록시 적용 대상 체크 : 앞서 3-1, 3-2에서 조회한 Advisor에 포함되어 있는 포인트컷을 사용해서 해당 객체가
프록시를 적용할 대상인지 아닌지 판단한다. 이때 객체의 클래스 정보는 물론이고, 해당 객체의 모든 메서드를 포인트컷에 하나하나 모두 매칭해본다.
그래서 조건이 하나라도 만족하면 프록시 적용 대상이 된다. 예를 들어서 메서드 하나만 포인트컷 조건에 만족해도 프록시 적용 대상이 된다.

5. 프록시 생성 : 프록시 적용 대상이면 프록시를 생성하고 프록시를 반환한다. 그래서 프록시를 스프링 빈으로 등록한다. 만약 프록시 적용 대상이 아니라면 원본 객체를 반환해서 원본 객체를 스프링 빈으로 등록한다.

6. 빈 등록 : 반한된 객체는 스프링 빈으로 등록된다.

