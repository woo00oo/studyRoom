## Executors

고수준(High-Level) Concurrency 프로그래밍

- 쓰레드를 만들고 관리하는 작업(try-catch 구문 등)을 애플리케이션에서 분리.

- 그런 기능을 Executors에게 위임.

### Executors가 하는 일

- 쓰레드 만들기 : 애플리케이션이 사용할 쓰레드 풀을 만들어 관리한다.

- 쓰레드 관리 : 쓰레드 생명 주기를 관리한다.

- 작업 처리 및 실행 : 쓰레드로 실행할 작업을 제공할 수 있는 API를 제공한다.

### 주요 인터페이스

- Executor : execute(Runnable)

- ExecutorService : Executor 상속 받은 인터페이스로, Callable도 실행할 수 있으며, Executor를 종료 시키거나, 여러 Callable을 동시에 실행하는 등의 기능을 제공한다.

- ScheduledExecutorService : ExecutorService를 상속 받은 인터페이스로 특정 시간 이후에 또는 주기적으로 작업을 실행할 수 있다.

#### ExecutorService로 작업 실행하기

```java


ExecutorService executorService = Executors.newSingleThreadExecutor();
executorSerivce.submit( () -> {
    System.out.println("Hello : " + Thread.currentThread().getName();
});

```

#### ExecutorServicee로 멈추기

```java

executorService.shutdown() // 처리중인 작업 기다렸다가 종료
executorService.shutdownNow () // 즉시 종료

```

### Fork/Join 프레임워크

- ExecutorService의 구현체로 손쉽게 멀티 프로세서를 활용할 수 있게끔 도와준다.