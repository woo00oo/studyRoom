## 자바 Concurrent 프로그래밍 소개

### Concurrent 소프트웨어

- 동시에 여러 작업을 할 수 있는 소프트웨어

- 예) 웹 브라우저로 유튜브를 보면서 키보드로 문서에 타이핑을 할 수 있다.

- 예) 녹화를 하면서 인텔리J로 코딩을 하고 워드에 적어둔 문서를 보거나 수정할 수 있다.

### 자바에서 지원하는 컨커런트 프로그래밍

- 멀티프로세싱(ProcessBuilder)

- 멀티쓰레드

### 자바 멀티 쓰레드 프로그래밍

- Thread / Runnable

#### Thread 상속

```java

public static void main(String[] args) {
    HelloThread helloThread = new HelloThread();
    helloThread.start();
    System.out.println("hello : " + Thread.currentThread().getName());
}

static class HelloThread extends Thread {
    @Override
    public void run() {
        System.out.println("world : " + Thread.currentThread().getName());
    } 
}

```

#### Runnable 구현 또는 람다

```java

Thread thread = new Thread(() -> System.out.println("world : " + Thread.currentThread().getName()));
thread.start();
System.out.println("hello : " + Thread.currentThread().getName());

```

