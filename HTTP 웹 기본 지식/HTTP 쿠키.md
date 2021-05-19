#### 쿠키

- set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답)

- Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

##### 쿠키의 사용 이유 - Stateless

- HTTP는 무상태(Stateless) 프로토콜이다.

- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.

- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.

- 클라이언트와 서버는 서로 상태를 유지하지 않는다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/243b4499-8e06-47f0-91bb-c224c79cf157/_2021-05-19__5.49.09.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210519%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210519T085330Z&X-Amz-Expires=86400&X-Amz-Signature=eede6a0e43bc7f0faf6a28acf79d3a24f46611134e011de4e536ae662e4b231e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2021-05-19__5.49.09.png%22)

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5ce1693b-31d9-4052-8193-a18590d97a02/_2021-05-19__5.54.35.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210519%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210519T085455Z&X-Amz-Expires=86400&X-Amz-Signature=3d49e19cbf1a9c81902a09773bd96239f0babf4a00aa11065ca73c4fb849a5eb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2021-05-19__5.54.35.png%22)

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4f7e53f1-3eb6-4488-91e6-b8e29d9264dc/_2021-05-19__5.55.45.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210519%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210519T085605Z&X-Amz-Expires=86400&X-Amz-Signature=bde3003a8ca46f587a54349dc7ab975651a6225f609a2faf58e5b915e28f25d8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2021-05-19__5.55.45.png%22)


- 예) set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure 

- 사용처

    - 사용자 로그인 세션 관리

    - 광고 정보 트래킹
    
- 쿠키 정보는 항상 서버에 전송됨

    - 네트워크 트래픽 추가 유발
    
    - 최소한의 정보만 사용(세션 id, 인증 토큰)

    - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고

- 주의 !

    - 보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드 번호 등등)
    

##### 쿠키 - 생명주기 Expires, max-age

- Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT

    - 만료일이 되면 쿠키 삭제
    
- Set-Cookie: max-age=3600 (3600초)

    - 0이나 음수를 지정하면 쿠키 삭제
    
- 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지

- 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지

##### 쿠키 - 도메인

- 예) domain-example.org

- 명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함
   
    - domain=exmple.org를 지정해서 쿠키 생성하면 example.org는 물론이고 dev.example.org로(하위 도메인)도 쿠키 접근.

- 생략 : 현재 문서 기준 도메인만 적용

    - example.org 에서 쿠키를 생성하고 domain 지정을 생략하면 exmple.org 에서만 쿠키 접근하고 dev.example.org(하위 도메인)는 쿠키 미접근.
    
##### 쿠키 - 경로 (Path)

- 예) path=/home

- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근

- 일반적으로 path=/ 루트로 지정

- Ex)

    - path=/home 지정
    
    - /home => 가능
    
    - /home/level1 => 가능
    
    - /home/level1/level2 => 가능
    
    - /hello => 불가능
    
##### 쿠키 - 보안 

- Secure

    - 쿠키는 http, https를 구분하지 않고 전송
    
    - Secure를 적용하면 https인 경우에만 전송
    
- HttpOnly

    - XSS 공격 방지
    
    - 자바스크립트에서 접근 불가(document.cookie)
    
    - HTTP 전송에서만 사용
    
- SameSite

    - XSRF 공격 방지
    
    - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송