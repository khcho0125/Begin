# @EnableGlobalMethodSecurity

### SpringSecurity 어노테이션 중 하나이다.

### SecurityConfig 설정 중 MethodSecurity를 사용하고 싶을 때 사용된다.

### 이 어노테이션의 파라미터는 아래와 같다.

* ### securedEnabled

  #### @Secured 어노테이션을 사용해 인가 처리를 하고 싶을 때 사용되는 옵션이다. 

* ### prePostEnabled

  #### @PreAuthorize, @PostAuthorize 어노테이션을 사용해 인가 처리를 하고 싶을 때 사용되는 옵션이다.

* ### jsr250Enabled

  #### @RolesAllowed 어노테이션을 사용하여 인가 처리를 하고 싶을 때 사용되는 옵션이다.