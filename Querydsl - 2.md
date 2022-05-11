# QueryDSL 정리

#### QueryDSL을 사용하려면 QueryConfig를 설정해준다.

![image](https://user-images.githubusercontent.com/82090641/161422591-862209e1-f1cf-4a24-8524-a2a5c29beed1.png)

## 사용방법

![image](https://user-images.githubusercontent.com/82090641/161424133-9aaae2ab-8f4c-44b1-99fb-0fc55b874693.png)

static 전역 변수로 import 받아 사용할 수 있다.

##### insert는 어째선지 작동하지 않고 원인 모를 오류가 생겨서 사용해보지 못했다.

##### 추후에 insert 오류 원인을 알아보도록 해야겠다...

##### 해보면서 느낀 점이지만 QueryDsl은 insert와 update가 효율적이진 않은 것 같다. Id도 IDENTITY로 만들어주는지 의문이다.

##### Insert는 대충 아래같은 방식으로 insert Query를 만든다고 이해만 하자

- ### INSERT

  #### 1. set 방식

  - Key, Value 형태로 하나씩 주입시키는 방식이다.

  ![image](https://user-images.githubusercontent.com/82090641/161424233-c90569bb-89af-4352-ad27-bd858b3f2040.png)

  #### 2. Values 방식

  - columns에 맞춰 values를 넘겨주는 방식이다.

  ![image](https://user-images.githubusercontent.com/82090641/161424684-2df3056d-d7e5-4a02-b70e-aa4223f5bc04.png)

- ### UPDATE

  - 동적 쿼리로 만든 update 과정이다.

  ![image](https://user-images.githubusercontent.com/82090641/161431621-fc015d69-a6da-4853-8c35-289f472f28d0.png)

  - set으로 쿼리와 동시에 연산 update

  1. money를 소모할 때
     - where로 id와 같은 멤버를 찾아 price를 빼준다.

  ![image](https://user-images.githubusercontent.com/82090641/161432044-3d5eda1d-b9dd-410d-a577-b0b531416322.png)

  1. 연관 관계 update

     - Entity를 select해서 대입해준다.


     - fetchOne : 하나의 객체를 반환

  ![image](https://user-images.githubusercontent.com/82090641/161432350-fdbce66b-71ee-4035-8583-34ba30f45dd5.png)

  ![image](https://user-images.githubusercontent.com/82090641/161432568-183ecf43-012d-464f-ade4-71eb4a42fcf8.png)

  1. 1:N 다중 매핑 update

     원래는 쿼리와 동시에 update 해볼려고 삽질을 해봤지만 많은 문제가 있어서 어쩔 수 없이 Entity를 가져와 업데이트 해주었다.

  ![image](https://user-images.githubusercontent.com/82090641/161433350-86527f5c-09ad-4b3d-b595-1c325bc087ee.png)

- ### SELECT

  QueryDSL의 가장 큰 장점이 SELECT를 할 때라고 느껴진다.

  아래는 가장 기본적인 Id로 찾는 쿼리

  ![image](https://user-images.githubusercontent.com/82090641/161453496-39ccfe74-4bb5-4fc3-a41f-0908f4aac2bd.png)

  ### List를 반환하는 쿼리

  - Join은 아래에서 정리하겠다.


  - fetch : 전체 객체를 반환
  - between : from과 to 사이의 값을 찾는다. ex) 5000 ≤ money ≤ 10000

  ![image](https://user-images.githubusercontent.com/82090641/161453810-4f42c2b3-1a3e-4109-9e81-ba5783ed0109.png)

  ### 같은 문자가 들어있는 객체를 반환하는 쿼리

  - like : str와 같은 문자일시 true

    "%str" : 뒤에 str이 붙은 문자

    "str%" : 앞에 str이 붙은 문자

    "%str%" : str이 포함된 문자

  ![image](https://user-images.githubusercontent.com/82090641/161453931-8f8b77ff-0a77-4362-9e74-5f41f3cf223a.png)

## QDto 객체를 만들고 싶을 때

쿼리값을 Dto로 받고 싶다면 사용하는 방법이다.

방법이 여러가지가 있는데 그 중 아래 3가지를 많이 사용한다.

- Projections.fields
- Projections.constructor
- @QueryProjection

나는 @QueryProjection을 사용하는 방법을 골랐다.

![image](https://user-images.githubusercontent.com/82090641/161454153-dd68a320-5d7b-47a8-a04c-392502fbde14.png)

아래와 같이 생성자에 @QueryProjection을 붙이면 아래와 같은 일이 벌어진다.

![image](https://user-images.githubusercontent.com/82090641/161454354-2d5d3a01-e673-4f4b-af15-ed8447028ed2.png)

위와 같이 QDto객체가 생성되었다!

이렇게 생성된 QDto객체를 가지고 아래와 같이 사용할 수 있다.

![image](https://user-images.githubusercontent.com/82090641/161454407-d34a7213-a558-4c0a-b667-57f5b32c1ce4.png)

QMemberVO를 통해 MemberVO 리스트를 만들었다.

![image](https://user-images.githubusercontent.com/82090641/161454600-0dba8529-e84a-466a-81a9-9084025d1890.png)

위는 1:1 관계를 가진 Dto를 생성하는 과정이다.

1:N 관계를 가진 Dto를 SELECT해 생성하기 위해서 많은 삽질을 해봤지만 List를 넘겨주는 과정을

구현할 수 없었다.. 방법을 계속 알아봐야겠다.

그래서 어쩔 수 없이 Entity를 가져와 Dto로 변환하는 과정을 사용했다.

![image](https://user-images.githubusercontent.com/82090641/161454706-c426feed-8b42-4bff-900b-acd9928c48ce.png)

아래는 1:N 관계를 가진 Dto List를 사용하는 방법이다.

![image](https://user-images.githubusercontent.com/82090641/161525009-f641d7a1-2163-44bb-8b96-1c6c087b3780.png)

leftJoin된 객체를 받으면 중복 값이 생기기 때문에 distinct로 중복을 제거한 다음 List를 변환해주었다.

- ### JOIN

  ##### 예시 테이블

  ### A Table

  |  ID  | NAME |
  | :--: | :--: |
  |  1   | AAA  |
  |  2   | BBB  |
  |  3   | CCC  |

  #### B Table

  |  ID  | NAME  |
  | :--: | :---: |
  |  1   |  ONE  |
  |  2   |  TWO  |
  |  4   | THREE |
  |  5   | FOUR  |

  - #### INNER JOIN

    ##### 공통적인 부분만 SELECT

    |  ID  | A.NAME | B.NAME |
    | :--: | :----: | :----: |
    |  1   |  AAA   |  ONE   |
    |  2   |  BBB   |  TWO   |

  - #### LEFT JOIN

    ##### 왼쪽에 있는 게 전부 SELECT

    |  ID  | A.NAME | B.NAME |
    | :--: | :----: | :----: |
    |  1   |  AAA   |  ONE   |
    |  2   |  BBB   |  TWO   |
    |  3   |  CCC   |  NULL  |

  - #### RIGHT JOIN

    ##### 오른쪽에 있는 게 전부 SELECT

    |  ID  | A.NAME | B.NAME |
    | :--: | :----: | :----: |
    |  1   |  AAA   |  ONE   |
    |  2   |  BBB   |  TWO   |
    |  4   |  NULL  | THREE  |
    |  5   |  NULL  |  FOUR  |

  - #### CROSS JOIN

    ##### 전부 SELECT

    |  ID  | A.NAME | B.NAME |
    | :--: | :----: | :----: |
    |  1   |  AAA   |  ONE   |
    |  2   |  BBB   |  TWO   |
    |  3   |  CCC   |  NULL  |
    |  4   |  NULL  | THREE  |
    |  5   |  NULL  |  FOUR  |

------

#### 이렇게 QueryDSL을 사용해 보았다.

- #### QueryDSL을 사용하면서 느낀점

  ##### SELECT를 하는 과정과 복잡한 쿼리를 만드는 작업이 재미있었다.

- ##### N + 1 문제를 해결하는 방법을 터득한 것 같아 흥미로웠다.

#### 참고 자료

https://tecoble.techcourse.co.kr/post/2021-08-08-basic-querydsl/

http://ojc.asia/bbs/board.php?bo_table=LecJpa&wr_id=348

https://devkingdom.tistory.com/245

https://backtony.github.io/jpa/2021-04-02-jpa-querydsl-3/
