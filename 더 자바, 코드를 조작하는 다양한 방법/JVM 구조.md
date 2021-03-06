## JVM 구조

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/50cf4208-9fbf-4dbf-92fc-2b3671d3721f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-09-20_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.51.27.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210920T025250Z&X-Amz-Expires=86400&X-Amz-Signature=92e3d4c2b2884f9c59d967a3aafce5eb7a96712dff0906cc52fbb9013122d595&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202021-09-20%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252011.51.27.png%22)

### 클래스 로더 시스템

- .class 에서 바이트코드를 읽고 메모리에 저장

- 로딩 : 클래스 읽어오는 과정

- 링크 : 레퍼런스를 연결하는 과정

- 초기화 : static 값들 초기화 및 변수에 할당

### 메모리 

- 메소드(method)영역에는 클래스 수준의 정보(클래스 이름, 부모 클래스 이름, 메소드, 변수)저장, 공유 자원이다.

- 힙 영역에는 객체를 저장, 공유 자원이다.

- 스택 영역에는 쓰레드 마다 런타임 스택을 만들고, 그 안에 메소드 호출을 스택프레임이라 부르는 블럭으로 쌓는다. 쓰레드 종료하면 런타임 스택도 사라진다.

- PC(Program Counter) 레지스터 : 쓰레드마다 쓰레드 내 현재 실행할 스택 프레임을 가리키는 포인터가 생성된다.

- 네이티브 메소드 스택 

### 실행 엔진

- 인터프리터 : 바이트 코드를 한줄 씩 실행.

- JIT 컴파일러 : 인터프리터 효율을 높이기 위해, 인터프리터가 반복되는 코드를 발견하면 JIT 컴파일러로 반복되는 코드를 모두 네이티브 코드로 바꿔둔다.
그 다음부터 인터프리터는 네이티브 코드로 컴파일된 코드를 바로 사용한다.

- GC(Garbage Collector) : 더 이상 참조되지 않는 객체를 모아서 정리한다.

### JNI(Java Native Interface)

- 자바 애플리케이션에서 C, C++, 어셈블리로 작성된 함수를 사용할 수 있는 방법 제공

- Native 키워드를 사용한 메소드 호출

- 네이티브 메소드 라이브러리(C, C++로 작성 된 라이브러리)