# OAuth 연동

### Kakao

1. Kakao Developers 에서 애플리케이션 추가

![image](https://user-images.githubusercontent.com/82090641/160951804-83c6aacf-8c96-49e0-b5eb-12c328ac023b.png)

2. 카카오 로그인을 활성화 후 개인정보 설정에서 닉네임, 프로필, 이메일을 가져온다.

![image](https://user-images.githubusercontent.com/82090641/160952419-10f436aa-d36e-41a7-af1b-29973d754e7d.png)

3. 정보가 반환될 RedirectURI을 설정한다.

![image](https://user-images.githubusercontent.com/82090641/160952667-00c134a6-f49d-4021-9ce0-c96e21a3fa83.png)

4. 앱설정 -> 플랫폼의 Web 설정을 한다.

![image](https://user-images.githubusercontent.com/82090641/160952878-eeb3a7cf-7741-4d01-ac58-85295a6f53ad.png)

5. application-oauth.yml

```yml
spring:
  security:
    oauth2:
      client:
        registration:
          kakao:
            redirect-uri: 카카오 Redirect URI
            client-id: REST API 키
            client-authentication-method: POST
            authorization-grant-type: authorization_code
            scope: profile_nickname, profile_image, account_email
            client-name: Kakao

        provider:
          kakao:
            authorization_uri: https://kauth.kakao.com/oauth/authorize
            token_uri: https://kauth.kakao.com/oauth/token
            user-info-uri: https://kapi.kakao.com/v2/user/me
            user_name_attribute: id
```

