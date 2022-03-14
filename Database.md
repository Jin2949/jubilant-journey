# Database

- csv파일 있는 곳에서 vscode실행
- sqlite3 tutorial.sqlite3
- .database   (를 하면 tutorial.sqlite3가 생성된다.)
- .mode csv
- .import hellodb.csv examples
- .tables
- SELECT * FROM examples;       (sql명령어 조회)
- .headers on
- SELECT * FROM examples;      (header가 뜬다)
- .mode column
- SELECT * FROM examples;        (column으로 뜬다.)

sqlite 확장프로그램 사용하기

- sql자동완성 하기 위해서 아래쪽 sqlite explorer에서 New Query를 누르고 저장을 하면 파일이 생성된다.
- 이 파일에서 똑같이 SQL문 작성하고 Run Query를 하면 옆에 데이터가 뜬다.
- tutorial.sqlite3 오른쪽 클릭하고 opendatabase



### 새로운 테이블 만들기

```python
SELECT * FROM examples;

CREATE TABLE classmates(id INTEGER PRIMARY KEY, name TEXT);  
----한줄 또는 엔터쳐서 만들어도 됨.
저장하고 SQLITE EXPLORER를 새로고침하면 classmates가 새롭게 만들어짐.확인가능

스키마 확인하는 법 쉘에서는
.schema classmates (ppt에서 확인)

```

### 테이블 삭제

- DROP TABLE classmates; 를하면 삭제 된다. 그리고 옆에서 새로고침해야 사라짐.

```python
CREATE TABLE classmates (
name TEXT,
age INT,
address TEXT
);
테이블 추가
```

여기까지가 테이블 만드는 것



## CREATE관련 명령

데이터 만들기

INSERT : 테이블에 단일 행 삽입

- 아래 tutorial.sqlite3 오른쪽 클릭 New Query (새로운 sql파일 생성)
- 01_create
- INSERT INTO classmates (name, age) VALUES ('홍길동', 23);
- Run Query 하고나서 EXPLORER에서 새로고침
- INSERT INTO classmates VALUES ('홍길동', 30, '서울');
- SELECT * FROM classmates;   ( 를 하면 조회하는데 옆에서 EXPLORER에서 확인)



### id는 보이지 않지만 알아서 관리해준다. 이것을 보기 위해서는 아래 코드를 치면된다.

SELECT rowid, * FROM classmates; 



NULL은 허용하지 않도록 하기 위해서는 스키마에서 NOT NULL을 구성해줘야 한다.

이전에 만들었던 TABLE삭제

- DROP TABLE classmates;

새롭게 NOT NULL로 구성 (아래코드)  !!! id는 분명하게 INTEGER로 쳐줘야함. 

- CREATE TABLE classmates (

  id INTEGER PRIMARY KEY,

  name TEXT NOT NULL,

  age INT NOT NULL,

  address TEXT NOT NULL

  );

- 위에서 실행했던 INSERT INTO classmates VALUES ('홍길동', 30, '서울'); 를 하면 

  ### 오류뜸!!

- id를 입력했기 때문에 id를 포함해서 작성해야한다.

- 두가지 방법으로 해결가능

  ```python
  1) INSERT INTO classmates VALUES (1, '홍길동', 30, '서울');
  2) INSERT INTO classmates (name, age, address) VALUES ('홍길동', 30, '서울');
  ```

id는 rowid로 해서 편하게 다룰 수 있도록 하자. 위와같은 문제때문에

위에서 썻던 코드들 sql파일에서 부분적으로 드래그해서 run 해서 사용가능

다시 id부분 지우고 새로만들기

DROP TABLE classmates;

- CREATE TABLE classmates (

  name TEXT NOT NULL,

  age INT NOT NULL,

  address TEXT NOT NULL

  );

value추가하기

```python
새로운 데이터 5개 추가하기 아래와 같은 모양으로 5번 치면 불편하다.
INSERT INTO classmates (name, age, address) VALUES ('홍길동', 30, '서울');

해결책!! VALUES에 값을 넣어준다.
INSERT INTO classmates (name, age, address) 
VALUES
('홍길동', 30, '서울'),
('철수', 29, '서울'),
('짱구', 26, '서울'),
('훈이', 24, '서울'),
('나', 20, '서울');
를 쳐서 value여러가지 추가
```



## READ배우기

### SELECT와 함께 사용하는 clause(절, 구문)  :

`ORDER BY, DISTINCT, WHERE, LIMIT, GROUP BY`

특정컬럼만 조회

`SELECT rowid, name FROM classmates;`

원하는 수 만큼 데이터 조회하기

`SELECT rowid, name FROM classmates LIMIT 1;`



특정 부분부터 원하는 수만큼 조회하기

`SELECT rowid, name FROM classmates LIMIT 1 OFFSET 2;`

OFFSET 2는 3번째 행부터 나오게 된다. 

ABCDEF 에서 위에코드 쓰면 A에서 2떨어진 C가 나온다.



WHERE절 (서울지역만 뽑기)

`SELECT rowid, name FROM classmates WHERE address ='서울';`



DISTINCT (나이 별로 나누기)

`SELECT DISTINCT age FROM classmates;`



## DELETE배우기

행지우기

`DELETE FROM classmates WHERE rowid=5;`

`INSERT INTO classmates VALUES ('최전자', 28, '부산');`

지웠다가 5를 다시만들면 sqlite는 재사용했다.

하지만 장고는 삭제하면 이유가 있어서 재활용하지않도록 아래의 값을 기본적으로 설정한다.

장고 SQL문 조회하면 AUTOINCREMENT 가 들어가있다. 



## UPDATE배우기

5번쨰꺼 주소바꾸기

`UPDATE classmates SET name='홍길동', address='제주도' WHERE rowid=5;`



TABLE에서는 CREATE, DROP을 쓰고

데이터에서 CRUD는 INSERT, SELECT, UPDATE, DELETE

오전정리

```python
#테이블 전체조회
SELECT * FROM 테이블이름; 
#테이블
#테이블 생성 (만들려는 데이터와 형식을 맞춰서 작성)
CREATE TABLE 테이블이름(
id INTEGER PRIMARY KEY,
name TEXT,
age INT,
address TEXT);
#테이블 삭제
DROP TABLE 테이블이름;

#데이터
#데이터 생성
INSERT INTO 테이블이름 (id, name,age, address) VALUES(1, '홍길동', 30, '서울');

#데이터 조회(여러가지 방식)
SELECT rowid, name FROM 테이블이름;
SELECT rowid, name FROM classmates LIMIT 2;
SELECT rowid, name FROM classmates LIMIT 1 OFFSET 2;
SELECT rowid, name FROM classmates WHERE address ='서울';
SELECT DISTINCT age FROM classmates;

#데이터 삭제
DELETE FROM classmates WHERE rowid=5;

#데이터 업데이트(내부변경, 원하는거 변경가능 )
UPDATE classmates SET 변경원하는 컬럼=값1 WHERE 조건;
UPDATE classmates SET address='제주도' WHERE rowid=5;
UPDATE classmates SET name='최전자' WHERE rowid=5;
UPDATE classmates SET name='최전자', address='제주도' WHERE rowid=5;

```



### 오후수업

#### 새로운 테이블 생성

```PYTHON
CREATE TABLE users ( first_name TEXT NOT NULL,
last_name TEXT NOT NULL,
age INTEGER NOT NULL,
country TEXT NOT NULL,
phone TEXT NOT NULL,
balance INTEGER NOT NULL);
```

- git bash에서 침.
- sqlite3 tutorial.sqlite3
- .tables
- .mode csv
- .import users.csv users
- .tables

### 다양한 것들로 조회하기

```python
SELECT * FROM users WHERE age >=30;
SELECT first_name FROM users WHERE age >= 30;
SELECT age, first_name FROM users WHERE age>=30 AND last_name='김';
```



### Aggregate function ("집계 함수")

`COUNT, AVG, MAX, MIN, SUM`  - 아래 count에 들어갈 수 있는 것

`SELECT COUNT(컬럼) FROM 테이블이름;` #총레코드수 조회



### LIKE

*,? 등을 사용하여 비슷한 문자 찾기

%, _: %는 들어올수 있고 없고, _필수로 들어와야함. 

##### 20대를 찾을 떄 쓰는 like

`SELECT * FROM users WHERE age LIKE '2_';`

##### 지역번호가 02인 사람 조회

`SELECT * FROM users WHERE phone LIKE '02-%';`

##### 이름이 준으로 끝나는 사람만 조회

`SELECT * FROM users WHERE first_name LIKE '%준';`

##### 중간번호가 5114인 사람만 조회

`SELECT * FROM users WHERE phone LIKE '%-5114-%'`



### ORDER BY

오름차순이 기본값(default)

##### users에서 나이 순으로 오름차순 정렬하여 상위 10개 (id순)

`SELECT * FROM users ORDER BY age ASC LIMIT 10;`

###### id도 확인할 수 있다 아래 코드로

`SELECT rowid,* FROM users ORDER BY age ASC LIMIT 10;`

##### 나이, 이름으로 정렬

`SELECT * FROM users ORDER BY age, last_name LIMIT 10;`



### GROUP BY clause

users에서 각 성씨가 몇 명씩 있는지 조회

`SELECT last_name, COUNT (*) FROM users GROUP BY last_name;`

last_name이 헤더 이름인데 AS로 바꿔줄 수 있다.

`SELECT last_name, COUNT(*) AS name_count FROM users GROUP BY last_name;`



### ALTER  TABLE

새로운 테이블 생성

`CREATE TABLE articles (title TEXT NOT NULL, content TEXT NOT NULL);`

`INSERT INTO articles VALUES ('1번제목', '1번내용');`

이름바꾸기

`ALTER TABLE articles RENAME TO news;`

새로운 컬럼 추가 (아래코드로 하면 오류뜸!!!)

`Alter TABLE news ADD COLUMN created_at TEXT NOT NULL;`

```python
해결방안 #오류가 뜨는 이유는 이전꺼에서 NULL값이 있기 때문이다
1) NOT NULL 설정 없이 추가하기
ALTER TABLE news ADD COLUMN created_at TEXT;

데이터 추가
INSERT INTO news VALUES('제목', '내용', datetime('now'));

2) DEFAULT 설정해주기
ALTER TABLE news ADD COLUMN subtitle TEXT NOT NULL DEFAULT '소제목';
```

테이블의 head 바꿔주기

`ALTER TABLE news RENAME COLUMN title TO main_title;`

테이블의 열 삭제 (최신문법)

`ALTER TABLE news DROP COLUMN subtitle;`

SQL문 ORM문 바꾸면서 사용할 수 있는지 중요함. 

