-----
### Oracle 명령어
-----
1. SQLPLUS :  SQL문을 실행시키고 그 결과를 볼수 있도록 오라클에서 제공하는 툴
  - SQL PLUS 명령어
      
        + 1. SET LINSIZE 크기 (너비 조정)
        + 2. SET PAGESIZE 크기 (페이지 조정)
	        예) SET LINESIZE 150 / SET PAGESIZE 500

2. USER-NAME : /as sysdba (데이터베이스 관리자로 연결)
3. CONN [/AS SYSDBA] : 데이터베이스 관리자로 접근
4. CONN [SYSTEM] : [시스템] 유저로 접근
5. SELECT * FROM ALL_USERS/DBA_USERS : 사용 유저 보기 
6. DBA_USERS : 모든 사용자 계정의 정보가 담긴 테이블
7. 특정 데이터베이스 내 모든 테이블 보기 :  SELECT t * FROM TAB;
8. 유저 삭제 : DROP USER 유저명;

9. 주의사항
- 오라클 접속 전 : sqlplus 계정명/비번
  + sys 계정 접근 : sqlplus "/as sysdba" (비밀번호 노출 방지를 위한 옵션)
    
- 오라클 접속 후 : conn 계정명/비번
  + scott 계정 접근 : conn scott/비밀번호
  + sys 계정 접근 : conn /as sysdba

------
10. hr계정 : 오라클이 교육용으로 만든 계정으로 기본적으로 잠겨있으므로, 계정 잠금 해제 필요
  - 계정해제 :  ALTER USER hr ACCOUNT UNLOCK;
  - 계정비밀번호 지정 : ALTER USER hr INDETIFIED BY 비밀번호;
  - CONN hr/hr(비밀번호) : hr 계정에 접근
-----
11. SPOOL
  - 시작 : SPOOL 경로/생성할 파일명
  - 종료 : SPOOL OFF (해당 지정파일에 저장)
  - BUFFER 내 / 의미 : 쿼리문의 ;와 동일하며, 버퍼에서는 오직 1개의 쿼리문만 실행가능
  - 버퍼 창 불러오기 : ed
  - 저장 후 실행 : 커멘드 창 : /
-----
12. RECYCLE_BIN TABLE : 삭제된 파일을 임시 보관하는 휴지통 테이블
    
   - OBJECT_NAME : 삭제된 테이블의 Object 이름 
   - ORIGINAL_NAME : 삭제된 테이블의 원래 이름 
   - TYPE : 타입 (예) 테이블, 인덱스 등)
  
   - 테이블 복구 : FLASHBACK TABLE "삭제된테이블의 OBJECT 이름" TO BEFORE DROP;
   - 테이블 완전 삭제 : DROP TALBE 테이블명 [PURGE];
     * purge : 완전 삭제
   - 휴지통 완전 삭제 : PURGE RECYCLEBIN;
-----
13. SQL 파일을 sqlplus에서 불러오기 : sqlpuls 실행 후, @파일주소\sql파일

-----
### 데이터의 구성
-----
1. Oracle에서 데이터는 데이터 파일과 로그 파일로 구성되어 데이터베이스에 저장
   - 데이터 파일 : 테이블의 데이터와 인덱스 데이터를 저장하는데 사용
   - 로그 파일 : 데이터베이스의 변경 사항을 기록

2. 데이터 파일 : 물리적으로 디스크에 저장
   - 데이터베이스는 데이터 파일을 관리하여 데이터의 안전성과 일관성을 유지
   - 데이터 파일은 세그먼트라는 단위로 나누어짐

-----
### 테이블 스페이스 (Table Space) 
-----
1. 데이터 저장 단위는 물리적, 논리적 단위로 나눌 수 있음
2. 물리적 단위는 파일이며, 논리적 단위는 데이터 블록 → 익스텐트 → 세그먼트 → 데이터 스페이스
3. 데이터 블록이 여러 개 모여 익스텐트를, 여러 개의 익스텐트가 모여 세그먼트를 구성
4. 테이블 스페이스는 테이블들을 담을 수 있는 커다란 공간이며, 실제 물리적으로 파일 존재
5. 테이블 스페이스 생성 방법
   - SQLPLUS 실행
   - system 계정으로 DBA에 접속(conn system/비밀번호)
   - 저장 영역에 새로운 테이블 스페이스를 만듬 (DBA - 저장영역 - 테이블 스페이스)
   - 테이블 스페이스에 접속할 수 있는 계정을 만들고, 테이블 스페이스와 연결 
   - 테이블 스페이스를 myts라는 이름으로 100MB 크기로 생성할 것이며, 데이터 늘어나 꽉 찰 것을 대비해 5MB씩 자동 증가 옵션 추가

      		CREATE TABLESPACE myts DATAFILE '생성위치\파일명' SIZE 100M AUTOEXTEND ON NEXT 5M;

6. 즉, Oracle에서 여러 테이블들을 하나의 그룹으로 묶어서 관리하는 개념 (즉, 데이터베이스에서 파일을 논리적으로 그룹화하는 방법)
7. 데이터 파일의 집합이며, 데이터베이스 객체(테이블, 인덱스 등) 저장하는데 사용
8. 데이터베이스의 성능, 관리 및 공간 할당을 제어하는 역할

-----
### 테이블 스페이스 (Table Space) 종류
-----
1. 기본 테이블스페이스 : 해당 사용자로 로그인한 뒤 테이블과 같은 각종 데이터베이스 객체가 저장되는 테이블 스페이스
2. 임시 테이블 스페이스 : 해당 사용자가 사용하는 디폴트 임시 테이블 스페이스
   - 정렬 작업 / 임시 테이블 생성 등과 같은 임시적 작업에 사용
   - 일시적으로 생성된 데이터가 저장
   - 성능 향상을 위해 따로 구성되며, 여러 사용자가 동시 접근 가능

3. 시스템 테이블 스페이스 : 데이터베이스의 시스템 관리에 필요한 정보를 저장하는데 사용되는 임시 테이블 스페이스
   - 데이터 사전(Data Dictionary)와 같은 시스템 객체 저장
   - 데이터베이스 생성 시 자동으로 생성

4. 사용자 테이블 스페이스 : 데이터베이스의 용량과 성능을 관리
   - 사용자가 생성한 테이블, 인덱스, 클러스터 등의 데이터를 저장하는데 사용
   - 데이터베이스 관리자가 직접 생성
   - 각 사용자에 대해 할당 가능
  
----
### 사용자 생성
----
1. 사용자 생성하기
   
           CREATE USER 유저명 IDENTIFIED BY 비밀번호
   	   DEFAULT TABLESPACE 테이블스페이스명
   	   TEMPORARY TABLESPACE 테이블스페이스명;

2, 권한 부여하기   

   - 사용자 생성을 완료하면, 해당 사용자에게 권한 부여
   - 데이터베이스 접속을 위해 CONNECT 라는 권한 부여해야 데이터베이스 접속 가능
   - GRANT DBA TO 유저명;

3. 사용자 계정 DB 접속 확인
   
   - CONNECT 유저명/암호;
   - SELECT USER FROM DUAL;
