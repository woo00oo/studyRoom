## Callable과 Future

### Callable

- Runnable과 유사하지만 작업의 결과(리턴값)을 받을 수 있다.

### Future 

- 비동기적인 작업의 현재 상태를 조회하거나 결과를 가져올 수 있다.

### 결과를 가져오기 get()

```java

ExecutorService executorService = Executors.newSingleThreadExecutor();

Future<String> helloFuture = executorService.submit(() -> {
    Thread.sleep(2000L);
    return "Callable";
});

System.out.println("Hello");

String result = helloFuture.get(); // 2초가 지난 후 결과값을 가져옴

System.out.println(result);

executorService.shutdown();



```

- 블로킹 콜이다

- 타임아웃(최대한으로 기다릴 시간)을 설정할 수 있다.

### 작업 상태 확인하기 isDone()

- 완료했으면 true 아니면 false를 리턴한다.

```java

ExecutorService executorService = Executors.newSingleThreadExecutor();

Future<String> helloFuture = executorService.submit(() -> {
    Thread.sleep(2000L);
    return "Callable";
});

Future<String> submit = executorService.submit(helloFuture);
submit.isDone();

```

### 작업 취소하기 cancel()

- 취소 했으면 true 못했으면 false를 리턴한다.

- parameter로 true를 전달하면 현재 진행중인 쓰레드를 interrupt하고 false를 전달하면 현재 진행중인 작업이 끝날때 까지 기다린다.

- 취소된 Future에 get() 하면 오류 발생. 

### 여러 작업 동시에 실행하기 invokeAll()

- 동시에 실행한 작업 중에 제이 오래 걸리는 작업 만큼 시간이 걸린다.

### 여러 작업 중에 하나라도 먼저 응답이 오면 끝내기 invokeAny()

- 동시에 실행한 작업 중에 제일 짧게 걸리는 작업만큼 시간이 걸린다.

- 블록킹 콜이다.

```java

//모든 작업의 결과 값을 List로 받아옴
List<Future<String>> futures = executorService.invokeAll(Arrays.asList(hello, java, hyeonWoo));


// 가장 빨리 끝나는 작업의 결과 값 타입으로 가져옴
String s = executorService.invokeAny(Arrays.asList(hello, java, hyeonWoo));

```