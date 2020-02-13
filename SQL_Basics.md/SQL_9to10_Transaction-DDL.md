# SQL Tutorial

> 배운 내용 중 핵심 내용만 정리하자



1. *데이터의 무결성과 트랜잭션*
2. *DDL : 테이블과 열 조작*



## 데이터 무결성과 트랜잭션

> 데이터베이스 시스템은 데이터에 접근하거나 처리할 때마다 부적절한 데이터가 입력되는 지 검사하여 데이터의 결점없음, 무결점을 유지한다.



#### 데이터 무결성의 종류

| 유형            | 설명                                                         |
| --------------- | ------------------------------------------------------------ |
| 개체 무결성     | 기본 키(primary key) 로 선택된 열은 고유해야 하고 null 값을 가질 수 없다. |
| 참조 무결성     | 참조하는 왜래 키(FK)가 존재하면 행 삭제 못한다.              |
| 영역 무결성     | 데이터의 형태, 범위, 기본값, 유일성에 관한 제한사항이다.(ex) 값이 0이상) |
| 비즈니스 무결성 | 업무에 따른 제약 조건                                        |



#### 제약 조건

1. 기본 키 제약 조건 : UNIQUE + NOT NULL 둘 다 만족 해야 한다.
2. 왜래 키 제약 조건 : 열 값이 부모 테이블의 참조 열의 값을 반드시 참조해야 한다.
3. 유일 키 : 중복된 값을 허용하지 않는다. 
4. NOT NULL : null 값을 허용하지 않는다.(반드시 값 입력)
5. CHECK : 범위나 조건 등 지정된 값만 허용.



#### 트랜잭션(Transaction)

데이터의 무결성이 보장되는 상태에서 DML 작업을 완수하기 위한 기본 작업단위. SELECT 문으로 데이터를 조회하고 DML을 실행하여 종료하는 과정까지를 말한다.



#### 트랜잭션의 특징(ACID)

| 개념   | 내용                                                         |
| ------ | ------------------------------------------------------------ |
| 원자성 | 트랜잭션의 처리가 완전히 끝난게 아니면 전혀 이루어 지지 않은 것과 같다. |
| 일관성 | 일관성이 보존된 상태                                         |
| 고립성 | 다른 트랜잭션의 부분적 실행결과를 볼 수 없다.                |
| 지속성 | 트랜잭션 성공하면, 결과를 영구적으로 보장해야한다.           |

1. 원자성 : 계좌이체 실패하면 안 한 것과 같다.
2. 일관성 :  A계좌 10만원 출금 == B계좌 10만원 입금
3. 고립성 : A계좌가 이체 완료 된 것이 아니면, B는 관여 못한다.
4. 보존성 : 계좌이체 성공 시, 결과 보장한다.



#### 트랜잭션의 수행 단계

| 상태                           | 설명                                        |
| ------------------------------ | ------------------------------------------- |
| 실행(active)                   | 트랜잭션을 실행 중                          |
| 부분 완료(partially committed) | DML등의 트랜잭션 명령 실행한 후 상태        |
| 완료(committed)                | 트랜잭션 성공적으로 완료                    |
| 실패(failed)                   | 더 이상 정상적으로 실행될 수 없음           |
| 철회(aborted== 롤백)           | 트랜잭션이 복원되어 수행 이전 상태로 돌아감 |



#### 트랜잭션 제어어(TCL, Transaction Control Language)

| 개념 | 설명                                            |
| ---- | ----------------------------------------------- |
| 커밋 | 트랜잭션의 모든 미결정 데이터를 영구적으로 반영 |
| 롤백 | 트랜잭션의 모든 미결정 데이터 변경을 포기       |



## DDL

> 테이블과 열을 조작하기 위한 언어 이며, 테이블과 관련 열을 생성&변경&삭제 하는 명령어를 데이터 정의어(DDL, Data Definition Language)라고 한다.



1. CREATE : 테이블 생성하기
2. ALTER : 테이블 수정하기
3. TRUNCATE : 테이블 내용 삭제 하기
4. DROP : 테이블 전체 삭제



#### CREATE : 테이블 생성하기

예제:

1. 테이블 생성

```sql
CREATE TABLE __테이블 이름__
	(__열이름__ number,
     __열이름__ vachar2(30),
     __열이름__ date
     );
```

2. 내용 넣기

```sql
INSERT INTO __테이블 이름__ VALUES (1, '__열이름__', to_date('140101', 'YYMMDD'));
INSERT INTO __테이블 이름__ VALUES (2, '__열이름__', to_date('150101', 'YYMMDD'));
INSERT INTO __테이블 이름__ VALUES (3, '__열이름__', to_date('160101', 'YYMMDD'));
commit;
```

- 테이블 중복 생성 불가하며, 예약어(SELECT, FROM, COUNT) 같은 언어는 사용 불가

#### ALTER : 테이블 수정하기

1. 열 추가 (기본값 null 인 열 생성했다.)

```sql
ALTER TABLE __테이블 이름__ ADD (__열이름__ varchar(10));
```

2. 열 수정(varchar타입 10자리를 char 타입으로 변경했다.)

```sql
ALTER TABLE __테이블 이름__ MODIFY (__열이름__ char(10));
```

3. 열 이름 변경

```sql
ALTER TABLE __테이블 이름__ RENAME COLUMN __열이름__ TO __new열이름__;
```

4. 열 삭제

```sql
ALTER TABLE __테이블 이름__ DROP COLUMN __열이름__;
```

#### TRUNCATE : 테이블 내용 삭제 하기

TRUNCATE 명령어는 테이블 내에 있는 데이터는 삭제하지만 테이블 자체는 삭제하지 않는다.(자동 커밋)

```sql
TRUNCATE TABLE __테이블 이름__
```



#### DROP : 테이블 전체 삭제

DROP 명령어는 테이블을 완전히 삭제 한다. 그와 연관된 모든 인덱스를 삭제한다.

```sql
DROP TABLE __테이블 이름__
```
