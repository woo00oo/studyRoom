## 도커 파일(Docker file)이란?

> 도커 이미지를 만들기 위한 설정 파일이며, 컨테이너가 어떻게 행동해야 하는지에 대한 설정들을 정의해 주는 곳.


## 도커 파일 만드는 순서(도커 이미지가 필요한 것이 무엇인지를 생각하기)

1. 베이스 이미지를 명시해준다. (파일 스냅샷에 해당)

2. 추가적으로 필요한 파일을 다운 받기 위한 몇가지 명령어를 명시해준다. (파일 스냅샷에 해당)

3. 컨테이너 시작시 실행 될 명령어를 명시해준다. (시작시 실행 될 명령어에 해당)

### 베이스 이미지는 무엇인가?

- 도커 이미지는 여러개의 레이어(중간 단계의 이미지)로 되어 있다.
그중에서 베이스 이미지는 이 이미지의 기반이 되는 부분이다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/18b9f552-5435-4cb5-ae17-3de5234dcf1f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-07-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.43.54.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210726%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210726T074411Z&X-Amz-Expires=86400&X-Amz-Signature=91041e8c256132998b5e839aa5a64a36393f3585dda7696f68068fb3089b15a4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-07-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.43.54.png%22)

## Dockerfile 문법

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5a7e9dd4-01a9-4453-afdc-f2ab974fe3ef/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-07-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.50.50.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210726%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210726T075737Z&X-Amz-Expires=86400&X-Amz-Signature=8111be5b0a30ad2b33dea14955b30cf5194038012beb5906a2effcf22fe3fffa&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2021-07-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.50.50.png%22)

FROM RUN CMD 등은 도커 서버에게 무엇을 하라고 알려주는 것이다.

#### FROM
이미지 생성시 기반이 되는 이미지 레이어.

<이미지 이름>:<태그> 형식으로 작성

태그를 안붙이면 자동적으로 가장 최신것으로 다운 받음

ex) ubuntu:14.04

#### RUN

도커 이미지가 생성되기 전에 수행할 쉘 명령어.

#### CMD

컨테이너가 시작되었을 때 실행할 실행 파일 또는 쉘 스크립트.

해당 명령어는 DockerFile 내 1회만 쓸 수 있음.