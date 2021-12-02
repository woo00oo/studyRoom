# Item 7 : 다 쓴 객체 참조를 해제하라

## 메모리 직접 관리

자바에 GC(가비지 컬렉터)가 있기 때문에 메모리 관리에 대해 신경쓰지 않아도 될거라고 생각하기 쉽지만 그렇지 않다.

```java
// 메모리 누수가 일어나는 위치는 어디인가?
public class Stack {

    private Object[] elements;

    private int size = 0;

    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack() {
        this.elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e) {
        this.ensureCapacity();
        this.elements[size++] = e;
    }

    public Object pop() {
        if (size == 0) {
            throw new EmptyStackException();
        }

        return this.elements[--size]; // 주목!!
    }

    /**
     * 원소를 위한 공간을 적어도 하나 이상 확보한다.
     * 배열 크기를 늘려야 할 때마다 대략 두 배씩 늘린다.
     */
    private void ensureCapacity() {
        if (this.elements.length == size) {
            this.elements = Arrays.copyOf(elements, 2 * size + 1);
        }
    }
}

```

*메모리 누수

이 스택을 사용하는 프로그램을 오래 실행하다 보면 점차 가비지 컬렉션 활동과 메모리 사용량이 늘어나 결국 성능이 저하될 것이다.

상대적으로 드문 경우긴 하지만 심할 때는 디스크 페이징이나 OutOfMemoryError를 일으켜 프로그램이 예기치 않게 종료되기도 한다.

스택이 커졌다가 줄어들었을 때 스택에서 꺼내진 객체들은 가비지 컬렉터가 회수하지 않는다. 프로그램에서 그 객체들을 더 이상 사용하지 않더라도 말이다.

이 스택이 그 객체들의 다 쓴 참조를 여전히 가지고 있기 때문이다. 여기서 다 쓴 참조란 문자 그대로 앞으로 다시 쓰지 않을 참조를 뜻한다.
앞의 코드에서는 elements 배열의 '활성 영역'밖의 참조들이 모두 여기에 해당한다. 활성 영역은 인덱스가 size보다 작은 원소들로 구성된다.

가비지 컬렉션 언어에서는(의도치 않게 객체를 살려두는) 메모리 누수를 찾기가 아주 까다롭다. 객체 참조 하나를 살려두면 가비지 컬렉터는 그 객체뿐 아니라 
그 객체가 참조하는 모든 객체(그리고 또 그 객체들이 참조하는 모든 객체..)를 회수해가지 못한다. 그래서 
단 몇개의 객체가 매우 많은 객체를 회수되지 못하게 할 수 있고 잠재적으로 성능에 악영향을 줄 수 있다.

해법은 간단하다. 해당 참조를 다 썼을 때 null 처리(참조 해제)하면 된다.

다음과 같이 코드를 수정할 수 있다
```java

public Object pop() {
   if (size == 0) {
        throw new EmptyStackException();
   }

    Object value = this.elements[--size];
    this.elements[size] = null;
    return value;
}

```

이에 따라, 만약 null 처리한 참조를 실수로 사용하려 하면 프로그램은 즉시 NullPointerException을 던지며 종료한다. 프로그램 오류는 가능한 조기에 발견하는 게 좋다.

그렇다고 필요없는 객체를 볼 때 마다 null로 설정하는 코드를 작성하지는 말자. 객체를 Null로 설정하는 건 예외적인 상황에서나 하는 것이지
평범한 일이 아니다.

다 쓴 참조를 해제하는 가장 좋은 방법은 그 참조를 담은 변수를 유효 범위(scope) 밖으로 밀어내는 것이다. 
변수의 범위를 최소가 되게 정의했다면 이 일은 자연스럽게 이뤄진다.

그렇다면 null 처리는 언제 해야 할까?

메모리를 직접 관리 할때, 이 스택은 (객체 자체가 아니라 객체 참조를 담는)elements 배열로 저장소 풀을 만들어 원소들을 관리한다.
배열의 활성 영역에 속한 원소들이 사용되고 비활성 영역은 쓰이지 않는다. 문제는 가비지 컬렉터는 이 사실을 알 길이 없다.

가비지 컬렉터가 보기에는 비활성 영역에서 참조하는 객체도 똑같이 유효한 객체다. 비활성 영역의 객체가 더 이상 쓸모 없다는 건 프로그래머만 아는 사실이다. 그러므로 
비활성 영역이 되는 순간 null 처리해서 해당 객체를 더는 쓰지 않을 것임을 가비지 컬렉터에 알려야 한다.

> 일반적으로 자기 메모리를 직접 관리하는 클래스라면 프로그래머는 항시 메모리 누수에 주의해야 한다.
> 원소를 다 사용한 즉시 그 원소가 참조한 객체들을 다 null 처리 해줘야 한다.

## 캐시

캐시 역시 메모리 누수를 일으키는 주범! 

캐시를 사용할 때도 메모리 누수 문제를 조심해야 한다. 객체의 레퍼런스를 캐시에 넣어 놓고 캐시를 비우는 것을 잊기 쉽다. 여러 가지 해결책이 있지만, 
캐시의 키에 대한 레퍼런스가 캐시 밖에서 필요 없어지면 해당 엔트리를 캐시에서 자동으로 비워주는 WeakHashMap을 쓸 수 있다.

또는 특정 시간이 지나면 캐시값이 의미가 없어지는 경우에 백그라운드 쓰레드를 사용하거나 
(아마도 ScheduledThreadPoolExecutor), 
새로운 엔트리를 추가할 때 부가적인 작업으로 기존 캐시를 비우는 일을 할 것이다. (LinkedHashMap 클래스는 removeEldestEntry라는 메서드를 제공한다.)

## 콜백

클라이언트가 콜백을 등록만 하고 명확히 해지하지 않는다면, 뭔가 조치해주지 않는 한 콜백은 계속 쌓여간다. 이럴 때 콜백을 약한 참조(weak reference)로 저장하면 가비지 컬렉터가 즉시 수거해 간다.

ex) WeakHashMap에 키로 저장.

> 메모리 누수는 겉으로 잘 드러나지 않아 시스템에 수년간 잠복하는 사례도 있다. 이런 누수는 철저한 코드 리뷰나 힙 프로파일러 같은 디버깅 도구를 동원해야만 발견되기도 한다. 그래서 이런 종류의 문제는 예방법을 익혀두는 것이 매우 중요하다.

### week reference

가비지 컬렉터의 대상의 되려면 해당 객체가 가리키는 레퍼런스가 모두 없어져야 한다.

```java
WeakReference weakWidget = new WeakReference(widget)
```
weak reference의 경우 Strong reference의 widget이 가비지 컬렉터의 대상이 되면 weakWidget 역시 가비지 컬렉터의 대상이 된다.

다시 한번 위의 캐시 내용을 정리 하자면,

```java

Object key1 = new Object();
Object value1 = new Object();

Map<Object, Object> cache = new HashMap<>(); 
cache.put(key1, value1);

```

HashMap인 경우 Strong 레퍼런스인 key1을 참조하고 있는 상황.

어디선가 key1에 대한 필요가 없어지더라도, cache 즉 Map 자체가 Strong 레퍼런스인 key1을 계속해서 참조하고 있기 때문에
key1의 객체는 가비지 컬렉터의 대상이 될 수 없다.

```java

Object key1 = new Object();
Object value1 = new Object();

Map<Object, Object> cache = new WeakHashMap<>(); 
cache.put(key1, value1);

```

하지만 WeakHashMap인 경우, Strong 레퍼런스인 key1이 필요 없어지더라도(Out of 스코프),
cache 즉 Map 자체가 가지고 있는 key1 객체를 가비지 컬렉터의 대상으로 만들 수 있다. 
WeakHashMap인 경우 key1를 weak 레퍼런스로 감쌀 것 이다! 


추후, Weak Reference에 대해 학습 해보자.

