# item5 : 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

많은 클래스가 하나 이상의 자원에 의존한다. 

ex) 아래 코드 예시.

가령 맞춤법 검사기는 사전(dictionary)에 의존하는데, 이런 클래스를 정적 유틸리티 클래스로 구현한 모습을 드물지 않게 볼 수 있다.

```java

// 부적절한 static 유틸리티 사용 예 - 유연하지 않고 테스트 할 수 없다.
public class SpellChecker {

    private static final Lexicon dictionary = new KoreanDicationry();

    private SpellChecker() {
        // Noninstantiable
    }

    public static boolean isValid(String word) {
        throw new UnsupportedOperationException();
    }


    public static List<String> suggestions(String typo) {
        throw new UnsupportedOperationException();
    }
}

interface Lexicon {}

class KoreanDicationry implements Lexicon {}


```

비슷하게, 싱글턴으로 구현하는 경우도 흔하다.

```java

// 부적절한 싱글톤 사용 예 - 유연하지 않고 테스트 할 수 없다.
public class SpellChecker {

    private final Lexicon dictionary = new KoreanDicationry();

    private SpellChecker() {
    }

    public static final SpellChecker INSTANCE = new SpellChecker() {
    };

    public boolean isValid(String word) {
        throw new UnsupportedOperationException();
    }


    public List<String> suggestions(String typo) {
        throw new UnsupportedOperationException();
    }

}


```

사전을 하나만 사용할꺼라면 위와 같은 구현도 만족스러울 수 있겠지만 그건 너무 순진한 생각이다.
실제로는 각 언어의 맞춤법 검사기는 사용하는 사전이 각기 다르다. 또한 테스트 코드에서는 테스용 사전을 사용하고 싶을 수도 있다.

> 사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.


클래스가 여러 자원 인스턴스를 지원해야 하며, 클라이언트가 원하는 자원(dictionary)을 사용해야 한다. 

-> 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식.

이는 의존 객체 주입의 한 형태로, 맞춤법 검사기를 생성할 때 의존객체인 사전을 주입해주면 된다.

```java

public class SpellChecker {

    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary) {
        this.dictionary = Objects.requireNonNull(dictionary);
    }

    public boolean isValid(String word) {
        throw new UnsupportedOperationException();
    }
    
    public List<String> suggestions(String typo) {
        throw new UnsupportedOperationException();
    }

}

class Lexicon {}

```

위와 같은 의존성 주입은 생성자, 정적 팩터리, 빌더 모두에 똑같이 응용할 수 있다.

### 정리

> 클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는 것이 좋다.
>이 자원들을 클래스가 직접 만들게 해서도 안 된다. 대신 필요한 자원을(혹은 그 자원을 만들어주는 팩터리를) 생성자에 (혹은 정적 팩터리나 빌더에)넘겨 주자.
>의존 객체 주입이라 하는 이 기법은 클래스의 유연성, 재사용성, 테스트 용이성을 기막히게 개선해준다.