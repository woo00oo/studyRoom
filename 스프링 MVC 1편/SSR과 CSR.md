## SSR - 서버 사이드 렌더링

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/39d96956-6a38-4cb7-8f11-710770f1da4d/_2021-05-30__11.09.41.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210530%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210530T141024Z&X-Amz-Expires=86400&X-Amz-Signature=f07ad7a2f132d3ed3d0026f37cf4a3590d9d8fddf2f33b0d87220e71954e21dc&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2021-05-30__11.09.41.png%22)

- 서버에서 최종 HTML을 생성해서 클라이언트에 전달

- 주로 정적인 화면에 사용

- 관련 기술: JSP, 타임리프

## CSR - 클라이언트 사이드 렌더링

- HTML 결과를 자바스크립트를 사용해 웹 브라우저에서 동적으로 생성해서 적용

- 주로 동적인 화면에 사용, 웹 환경을 마치 앱 처럼 필요한 부분부분 변경할 수 있음

- ex) 구글 지도, Gmail, 구글 캘린더

- 관련 기술: React, Vue.js

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fb2b0efd-dbad-4085-a98d-80827f2201d0/_2021-05-30__11.11.30.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210530%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210530T142006Z&X-Amz-Expires=86400&X-Amz-Signature=c3f186867b775c746ca6557b0d41c3705e9bb88545ad1a594eeb05172c1def4a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2021-05-30__11.11.30.png%22)

---

| | SSR | CSR|
| --- | --- | ---|
| 초기 로딩 속도 | CSR에 비해 다운 받는 파일이 많지가 않아 속도가 빠르다. | 모든 JS 파일을 다운 받아와야 하기 때문에, 초기에는 오래 걸린다.|
| 서버 부담 | 서버와 잦은 응답(view 변경 시 마다)을 하기 때문에 서버에 부담이 된다. | data 요청이 있을 때만 서버에 요청하기 때문에 서버에 부담이 적다.|
| SEO(검색 엔진 최적화)| HTML에 대한 정보가 처음에 포함되어 있어 데이터를 수집할 수 있다. | 맨 처음 HTML 파일이 비어 있어, 크롤러가 데이터를 수집할 수 없다.(단, 구글 제외)|