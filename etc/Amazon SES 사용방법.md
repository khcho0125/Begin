# Amazon SES 사용방법

#### 인증되지 않은 이메일로 메일 발송을 하려면 Amazon SES를 샌드박스 환경 외부로 나가야한다.

* ### 샌드박스란? 

  #### Amazon SES가 기본적으로 작동하는 환경이다.

  #### 샌드박스란 고객을 도용이나 침해로부터 보호하고 이메일 수신자에게 신뢰를 줄 수 있도록 제약 사항을 두는 환경을 말한다.

  #### 즉, 샌드박스에 등록된 이메일만 전송할 수 있다는 것이다.

  #### 그러므로 인증되지 않은 이메일로 발송을 하려면 샌드박스에서 벗어나야 한다.

#### Amazon SES console에 들어가면 샌드박스 해제를 요청할 수 있을 것이다.

#### 요청 후 성공적으로 샌드박스를 해제했다는 샌드박스 해제 및 일일 전송 할당량 증가가 되었을 것이다. 

<img src="https://user-images.githubusercontent.com/82090641/172019205-b782c377-adb6-4dc5-9b8c-165098858749.png">

#### 이제 인증되지 않은 이메일에도 메일 발송을 할 수 있을 것이다!

---

* ### Amazon SES 내부에서 이메일 템플릿을 저장해두는 방법을 알아보도록 하겠다.

#### Amazon SES Configuration에서 Email templates에 들어가보자.

#### All templates는 비어있을 것이다. 그런데 Create 버튼이 없다?! 

### 그러면 템플릿을 어떻게 만드는가?

#### 템플릿을 만들어 저장하는 방법은 AWS CLI에서 추가해주는 방법, 프로젝트에서 Template를 넣어주는 방법 등이 있다.

* ### AWS CLI

  #### 먼저 CLI에서 템플릿을 추가하는 방법을 알아보자

  ```powershell
  $ aws configure
  ```

  #### aws configure에서 SES 계정 AccessKey와 SecretKey, Region을 설정해준다.

  ```powershell
  $ aws ses list-templates
  ```

  #### 위와 같이 입력하면 TemplatesMetadata 리스트가 보일 것이다.

  ```powershell
  $ vi mytemplate.json
  ```

  #### 텍스트 편집기로 json 파일을 만들겠다. 아래 코드와 같은 형식으로 붙여 넣는다.

  ```json
  {
    "Template": {
      "TemplateName": "MyTemplate",
      "SubjectPart": "Greetings, {{name}}!",
      "HtmlPart": "<h1>Hello {{name}},</h1><p>Your favorite animal is {{favoriteanimal}}.</p>",
      "TextPart": "Dear {{name}},\r\nYour favorite animal is {{favoriteanimal}}."
    }
  }
  ```

  ```powershell
  $ aws ses create-template --cli-input-json file://mytemplate.json
  ```

  #### 위 명령을 사용하고 다시 TemplatesMetadata 리스트를 확인하면 위 템플릿이 저장되었을 것이다.

  <img src="https://user-images.githubusercontent.com/82090641/172019923-11917e57-90b2-4ac0-a769-5bd92373ca61.png">

* ### SES Dependencies

  #### 프로그램의 의존성을 이용하여 템플릿을 만들어 저장할 수 있다.

  #### AWS SimpleEmailService 설정 후 템플릿을 넣어 주겠다. 

  * ##### AWS Config

  ```java
      @Bean
      public AmazonSimpleEmailService amazonSimpleEmailService() {
          BasicAWSCredentials basicAWSCredentials = new BasicAWSCredentials(accessKey, secretKey);
          AWSStaticCredentialsProvider awsStaticCredentialsProvider = new AWSStaticCredentialsProvider(basicAWSCredentials);

          return AmazonSimpleEmailServiceClient.builder()
                  .withCredentials(awsStaticCredentialsProvider)
                  .withRegion(region)
                  .build();
      }
  ```

  * ##### AWS SES CreateTemplate

  ```java
      public void Template(String HtmlTemplate) {
          Template template = new Template();
          template.setTemplateName("Test");
          template.setSubjectPart("AWS SES에 오신 것을 환영합니다.");
          template.setTextPart(null);
          template.setHtmlPart(HtmlTemplate);

          CreateTemplateRequest request = new CreateTemplateRequest().withTemplate(template);
          amazonSimpleEmailService.createTemplate(request);
      }
  ```

  #### 위와 같이 CreateTemplateRequest를 사용해 템플릿을 저장할 수 있다.

* #### 등록된 템플릿

<img src="https://user-images.githubusercontent.com/82090641/172020094-6e7db798-0e91-4dfb-ace5-2ed7251511eb.png">

* #### 템플릿 테스트

  <img src="https://user-images.githubusercontent.com/82090641/172020553-72f2b1f3-b2f0-420e-b8e3-a8a6cada0b80.png">
