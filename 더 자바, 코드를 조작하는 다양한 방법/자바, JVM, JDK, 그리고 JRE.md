## 자바, JVM, JDK, JRE

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/38c2d928-c0b8-44e8-86fd-f02e5d6c8b3d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-09-20_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.35.15.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210920T013544Z&X-Amz-Expires=86400&X-Amz-Signature=af7ef40d8da3f720535e22ab13884badf39a3149b9b83ae41afba66a01e31c1b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-09-20%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252010.35.15.png%22)

### JVM(Java Virtual Machine)

- 자바 가상 머신으로 자바 바이트 코드(.class 파일)를 OS에 특화된 코드로 변환(인터프리터와 JIT 컴파일러)하여 실행한다.

- 바이트 코드를 실행하는 표준(JVM 자체는 표준)이자 구현체(특정 밴더가 구현한 JVM)이다.

- JVM 밴더 : 오라클, 아마존, Azul, ....

- 특정 플랫폼에 종속적.

### JRE(Java Runtime Environment) : JVM + 라이브러리

- 자바 애플리케이션을 실행할 수 있도록 구성된 배포판.

- JVM과 핵심 라이브러리 및 자바 런타임 환경에서 사용하는 프로퍼티 세팅이나 리소스 파일을 가지고 있다.

- 개발 관련 도구는 포함하지 않는다(그건 JDK에서 제공)

### JDK(Java Development Kit) : JRE + 개발 툴

- JRE + 개발에 필요할 툴

- 소스 코드를 작성할 때 사용하는 자바 언어는 플랫폼에 독립적

- 오라클은 자바 11부터는 JDK만 제공하며 JRE를 따로 제공하지 않는다.(그전에는 JDK, JRE 각각 따로 제공했었음)

### 자바

- 프로그래밍 언어

- JDK에 들어있는 자바 컴파일러(javac)를 사용하여 바이트코드(.class 파일)로 컴파일 할 수 있다.

- 자바 유료화? 오라클에서 만든 Oracle JDK 11 버전부터 상용으로 사용할 때 유료.

### JVM 언어

- JVM 기반으로 동작하는 프로그래밍 언어

- 클로저, 그루비, JRuby, Jython, Kotlin, Scala, ...