-----
### INSEERT
-----
1. 테이블에 새로운 Row(Tuple, Record)을 추가하는 구문
2. INSERT INTO TABLE_NAME (COL_NAME1, COL_NAME2 ... ) VALUES (VALUE1, VAULE2, ...)
   - COL_NAME1과 VALUE1, 이처럼 서로가 1:1로 매칭
    
3. 다음과 같이 EMP01 TABLE과 구조가 존재한다고 하자.
```sql
CREATE TABLE EMP01
AS
SELECT EMPNO, ENAME, JOB
FROM
WHERE 1=0; (조건이 거짓이므로, 데이터는 가져오지 못해도 속성은 가져옴)
```

```sql
EMPNO NUMBER(4, 0)
ENAME VARCHAR(10)
JOB VARCHAR2(9)
```

4. 다음과 같은 사원 정보를 입력한다.
   - 사원번호가 1111, 이름이 홍길동, 직무가 인사인 사원의 정보를 입력
```sql
INSERT INTO EMP01(EMPNO, ENAME, JOB) VALUES(1111, '김길동', '인사');
```

  - 사원번호가 2222, 이름이 김길동, 직무가 개발인 사원의 정보를 입력
```sql
INSERT INTO EMP01(EMPNO, ENAME, JOB) VALUES (2222, '김길동', '개발');
```

  - 사원번호가 3333, 이름이 최길동, 직무가 인사인 사원의 정보를 입력
```sql
INSERT INTO EMP01(EMPNO, ENMAE, JOB) VALUES(3333, '최길동', '인사');
```

  - 사원번호가 4444, 이름이 박길동, 직무가 생산인 사원의 정보를 입력
```sql
INSERT INTO EMP01(EMPNO, ENAME, JOB) VALUES(4444, '박길동', '생산');
```

-----
### INSEERT : COLUMN 목록을 생략하는 경우
-----
1. INSERT INTO TABLE_NAME VALUES (VALUE1, VAULE2, ...) : 생략되어도 VALUE 항목대로 각 테이블 항목에 1:1매칭

2. 다음과 같은 사원 정보를 입력
   - 사원번호가 5555, 이름이 황길동, 직무가 개발인 사원의 정보를 입력
```sql
INSERT INTO EMP01 VALUES(5555, '황길동', '개발');
```

-----
### INSEERT : COLUMN 목록에 모든 목록이 있지 않은 경우
-----
1. 다음과 같은 사원 정보를 입력
   - 사원번호가 6666, 이름이 이길동인 사원의 정보를 입력
```sql
INSERT INTO EMP01 VALUES(6666, '이길동');
```
  : JOB Attribute에는 NULL 값이 입력   

2. 지정해주지 않으면 NULL 값이 삽입되며, 해당 속성 값이 필요하지 않으면 NULL값 부여 가능
   - 사원번호가 7777, 이름이 고길동인 사원의 정보를 입력
```sql
INSERT INTO EMP01 VALUES(7777, '고길동', NULL);
```

-----
### INSEERT : Sub-Query로 데이터 저장
-----
```sql
INSERT INTO TABLE_NAME Sub-Query;
```
-----
```sql
INSERT ALL
INTO TABLE_NAME(COL_NAME) VALUES(COL_NAME)
INTO TABLE_NAME(COL_NAME) VALUES(COL_NAME);
```

1. 다음과 같이 EMP01 TABLE과 구조가 존재한다고 하자.
```sql
CREATE TABLE EMP01
AS
SELECT EMPNO, ENAME, JOB
FROM EMP
WHERE 1=0;
```

```sql
EMPNO NUMBER(4, 0)
ENAME VARCHAR(10)
JOB VARCHAR2(9)
```

2. Sub-Query를 통한 데이터 저장 예
```sql
INSERT INTO EMP01(EMPNO, ENAME, JOB)
SELECT EMPNO, ENAME, JOB
FROM EMP;
```