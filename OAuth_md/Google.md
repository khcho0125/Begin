# Oauth 연동

Oauth 플랫폼 연동 방법 정리

Oauth2 지원 플랫폼

### 1. Google
### 2. Kakao
### 3. Naver
### 4. Github
### 5. FaceBook

## Google

1. 구글 클라우드 플랫폼 사이트에서 프로젝트 생성

![화면 캡처 2022-03-29 154813](https://user-images.githubusercontent.com/82090641/160550570-2a5ce733-76fb-43f4-b9b5-a3bace0ac2a4.png)

2. 탐색 메뉴 > API 및 서비스 > 사용자 인증 정보 만들기

여기서 클라이언트와 서비스 계정을 만들어준다.

![image](https://user-images.githubusercontent.com/82090641/160550945-c06cf7df-82fa-409a-afe2-d1a16e63b45f.png)

3. OAuth 동의 화면

앱 이름과 사용자 지원 이메일, 개발자 연락처 정보를 입력한다.

Google API 범위를 email, profile, openid로 지정한다.

4. OAuth Client Id 만들기

클라이언트 ID 이름과 데이터가 전송될 리다이렉션 URI를 적어준다.

![image](https://user-images.githubusercontent.com/82090641/160552202-9296408e-ed52-45ca-a852-e0e52401636e.png)

5. spring boot 설정파일

application-oauth.properties or yml 파일에 아래 내용을 작성한다.

* properties
```properties
spring.security.oauth2.client.registration.google.client-id=클라이언트 ID
spring.security.oauth2.client.registration.google.client-secret=클라이언트 보안 비밀번호
spring.security.oauth2.client.registration.google.scope=profile,email
```

* yml
```yml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: 클리이언트 ID
            client-secret: 클라이언트 보안 비밀번호
            scope: profile,email
```

#### application 파일

```yml
spring:
  profiles:
    include: oauth
```
