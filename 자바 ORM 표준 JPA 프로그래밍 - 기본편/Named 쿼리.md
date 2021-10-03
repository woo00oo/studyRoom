### Named 쿼리

- 미리 정의해서 이름을 부여해두고 사용하는 JPQL

- 정적 쿼리만 가능

- 어노테이션, XML 정의

- XML이 항상 우선권을 가진다.

- 애플리케이션 운영 환경에 따라 다른 XML을 배포할 수 있다.

- 애플리케이션 로딩 시점에 초기화 후 재사용

- 애플리케이션 로딩 시점에 쿼리를 검증(쿼리 문법이 잘못 되었을 경우, 컴파일 시점에서 확인이 가능하다)

- spring data jpa를 사용하는 경우 JpaRepository를 상속 받은 인터페이스에서 정의가 가능하다.

Ex)

```java


public interface UserRepository extends JpaRepository<User, Long> {


    @Query("select u from User u where u.emailAddress = ?1") //Namde 쿼리와 같은 기능.
    User findByEmailAddress(String emailAddress);

}



```