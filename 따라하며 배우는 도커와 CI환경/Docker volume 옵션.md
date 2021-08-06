### Volume은 무엇일까?

> 볼륨은 도커 컨테이너에 의해 생성되고 사용되는 지속적인 데이터를 위한 선호 메커니즘.


### Volume의 사용.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8fc35f11-d8af-4550-b734-0a4c1edd1497/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.48.12.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210806%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210806T051717Z&X-Amz-Expires=86400&X-Amz-Signature=335959efc9f64333a65304c54c42b04107fd6fc8888bdee2113004ea86038baf&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_1.48.12.png%22)


#### 1.
> 로직 코드가 업데이트 될때 바로 수정된 코드가 도커 컨테이너에 올라간 어플리케이션에 적용하기
위해서 사용.

(volume 옵션을 사용하지 않으면 컨테이너를 내리고 이미지를 다시 빌드 시켜야 한다. -> 번거로움)

#### 2.

> 데이터베이스에 저장된 자료를 컨테이너를 지우더라도 자료가 지워지지 않을 수 있게 하기 위해서 사용.

원래는 다음과 같이 컨테이너를 지우면 컨테이너에 저장된 데이터들이 지워지게 된다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/394e0209-8d3c-461f-8d05-7b7e823170eb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.15.24.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210806%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210806T051734Z&X-Amz-Expires=86400&X-Amz-Signature=d9a88781792f349d3a0d75aef988d6623ceef1ec8a633488fc1abfc15e6a83f4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.15.24.png%22)

하지만 volume 옵션을 사용하게 되면 컨테이너에서 변화가 일어난 데이터가 컨테이너안에 저장되는 것이 아닌 
호스트 파일 시스템에 저장되고 그 중에서도 도커에 의해서만 통제가 되는 도커 Area에 저장이 되므로 컨테이너를 삭제해도
변화된 데이터는 사라지지 않는다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2e196f23-e038-434d-b17c-f153ef3e9585/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.53.53.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210806%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210806T051756Z&X-Amz-Expires=86400&X-Amz-Signature=3a4631300fe4076cf92085de7a493c6bb6cca94c4f15ae727f422d97f036da97&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_1.53.53.png%22)