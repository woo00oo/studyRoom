### Dockerfile 하나인 경우.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d0cbdddf-7d37-472c-9665-a7b0030cb687/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.29.41.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210811%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210811T024717Z&X-Amz-Expires=86400&X-Amz-Signature=395a20f29bc3fa63a13f8a794917e36b1ef746bc9485bfebf2e81fba95aca70c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.29.41.png%22)

도커 파일이 하나일 경우에는 Elastic beanstalk에 전달하면 EB가 알아서 그 빌드된 이미지를 돌려서 어플리케이션을 실행함.
(아무런 설정을 해주지 않아도 됨)


### Dockerfile 여러개 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a6df685c-9d16-4bd3-92c9-c7f161085779/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.08.26.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210811%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210811T030913Z&X-Amz-Expires=86400&X-Amz-Signature=50c7c43c639bd127a3da823675e43f88b72c855349a3b553c7bb6be042a0931c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_12.08.26.png%22)

노드, mysql, 엔진엑스등을 위한 Dockerfile 이 여러개 이기 때문에 EB가 어떤 파일을 먼저 실행하고
어떻게 행동을 취해야 하는지 자동으로 프로세스를 해나갈 수 없기 때문에 임의로 설정을 해줘야 한다.

=> 그 설정을 해주는 파일이 바로 Dockerrun.aws.json 이다

### AWS에서 말하는 Dockerrun.aws.json 파일의 정의.

Dockerrun.aws.json 파일은 Docker 컨테이너 세트를 Elastic Beanstalk 애플리케이션으로 하는 방법을 설명하는
Elastic Beanstalk 고유의 JSON 파일이다. 

Dockerrun.aws.json 파일을 멀티컨테이너 Docker 환경에 사용할 수 있다.

Dockerrun.aws.json은 환경에서 각 컨테이너 인스턴스(Docker 컨테이너를 호스트하는 Amazon Ec2 인스턴스)에 배포할 
컨테이너 및 탑재할 컨테이너의 호스트 인스턴스에서 생성할 데이터 볼륨을 설명한다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/977d2f68-8788-4e4d-8ced-19535615e25c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.02.37.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210811%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210811T031059Z&X-Amz-Expires=86400&X-Amz-Signature=f12472baa6988e0207a2b27e218586fac5de9b757dd5003ac7820bcd101101e0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_12.02.37.png%22)


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/17603db1-2e01-4634-b89e-7a9446ecd9a8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.11.33.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210811%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210811T031224Z&X-Amz-Expires=86400&X-Amz-Signature=1ff529bc2d196488b7d1fed16a4597d2f8711ecc249753a1d7d2b91bbb67803f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_12.11.33.png%22)

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/74bb8de2-9637-46ef-9b64-5aef81ca2b41/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.11.55.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210811%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210811T031313Z&X-Amz-Expires=86400&X-Amz-Signature=299310253a23ceee43bd77a31cdaa465b4895e5e50f14c32ff3e63e293f98b04&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_12.11.55.png%22)


#### Task Definition(작업 정의)에 지정할 수 있는 것들

- 작업의 각 컨테이너에 사용할 도커 이미지

- 각 작업 또는 작업 내 각 컨테이너에서 사용할 CPU 및 메모리의 양

- 사용할 시작 유형으로서 해당 작업이 호스팅되는 인프라를 결정

- 작업의 컨테이너에 사용할 도커 네트워킹 모드

- 작업에 사용할 로깅 구성

- 컨테이너가 종료 또는 실패하더라도 작업이 계속 실행될지 여부

- 컨테이너 시작 시 컨테이너가 실행할 명령

- 작업의 컨테이너에서 사용할 데이터 볼륨

- 작업에서 사용해야 하는 IAM 역할


이러한 작업 정의를 등록하려면 Container Definition 을 명시해줘야한다. 
그리고 그 Container Definition 은 dockerrun.aws.json에 명시해주며 도커 데몬으로 전해진다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4917b415-edb3-49ad-8075-ce86a189f265/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.18.49.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210811%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210811T032058Z&X-Amz-Expires=86400&X-Amz-Signature=bbc5b97af8215dba183dbf8a4a36c5c376d740603eba5b353c50de4719d80278&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-08-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_12.18.49.png%22)


#### Container Definitions 작성

AWSEBDockerrunVersion : Dockerrun 버전 2로 지정

containerDefinitions : 이 안에서 컨테이너들을 정의 해줌.

name : 컨테이너의 이름

image : Docker 컨테이너를 구축할 도커 허브의 Docker 이미지 이름

hostname : 호스트 이름. 이 이름을 이용해서 도커 컴포즈를 이용해 생성된 다른 컨테이너에서 접근이 가능.

essential : 컨테이너가 실패할 경우 작업을 중지해야 하면 true. 필수적이지 않은 컨테이너는 인스턴스의 나머지 컨테이너에 영향을 미치지 않고 종료되거나 충돌할 수 있다.

memory : 컨테이너용으로 예약할 컨테이너 인스턴스에 있는 메모리 양. 컨테이너 정의에서 memory 또는 memoryReservation 파라미터 중 하나 또는 모두에 0이 아닌 정수를 지정.

portMappings : 컨테이너에 있는 네트워크 지점을 호스트에 있는 지점에 매핑.

links : 연결할 컨테이너의 목록. 연결된 컨테이너는 서로를 검색하고 안전하게 통신할 수 있음.