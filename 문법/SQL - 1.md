# SQL 문법 공부 정리

1. SELECT FROM WHERE

   다음 작가들이 쓴 책들만 골라서 조회

   - William Shakespeare
   - John Ronald Reuel Tolkien
   - Joanne Kathleen Rowling

   SELECT * FROM book WHERE author = 'William Shakespeare' or author = 'John Ronald Reuel Tolkien' or author = 'Joanne Kathleen Rowling'

   ### 최적화

   - SELECT FROM WHERE IN

​	SELECT * FROM book WHERE author in ('William Shakespeare', 'John Ronald Reuel Tolkien', 'Joanne Kathleen Rowling')



1. SELECT FROM WHERE BETWEEN

   가격이 5000 ~ 7000원 사이인 물품을 모두 조회

   SELECT * FROM product WHERE price BETWEEN 5000 AND 7000



1. SELECT FROM WHERE LIKE

   다음 문자가 이름에 포함된 멤버들을 조회

   - 문자 = '재'

   SELECT * FROM MEMBER WHERE name LIKE '%재%'

   - '%문자' 문자로 끝남

   - '문자%' 문자로 시작

   - '%문자%' 문자가 포함됨

     ​

2. UPDATE SET WHERE

   당신이 상점에서 500원을 주고 물건을 샀을 때 수정 (id = 123)

   UPDATE USER SET money = money - 500 WHERE id = 123



1. DELECT FROM WHERE 

   id가 5인 멤버 삭제

   DELECT FROM MEMBER WHERE id = 5
   
   
