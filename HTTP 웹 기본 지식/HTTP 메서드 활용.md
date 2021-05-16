#### HTTP API 설계 예시

1. HTTP API - 컬렉션
    - POST 기반 등록
    
    - 클라이언트는 등록될 리소스의 URI를 모른다.
    
        - 회원 등록 /members -> POST
        
        - POST /members
    
    - 서버가 새로 등록된 리소스 URI를 생성해준다.
    
        - HTTP/1.1 201 Created \
        Location: /members/100
        
    - 컬렉션(Collection)
        
        -서버가 관리하는 리소스 디렉토리
        
        - 서버가 리소스의 URI를 생성하고 관리
        
        - 여기서 컬렉션은 /members
    
    ex) 회원 관리 API 제공
    
2. HTTP API - 스토어

    - PUT 기반 등록
    
    - 클라이언트가 리소스 URI를 알고 있어야 한다.
    
        - 파일 등록 /files/{filename} -> PUT
        
        - PUT /files/star.jpg
       
    - 클라이언트가 직접 리소스의 URI를 지정한다.
    
    - 스토어(Store)
    
        - 클라이언트가 관리하는 리소스 저장소
        
        - 클라이언트가 리소스의 URI를 알고 관리
        
        - 여기서 스토어는 /files
    
    ex) 정적 컨텐츠 관리, 원격 파일 관리
    
> 대부분 자원 등록은 POST! 컬렉션