# SQL Tutorial

> 배운 내용 중 핵심 내용만 정리하자



#### 관계형 데이터베이스의 주요 지식

1. *뷰 View*
2. *최적화 Optimizer*
3. *인덱스 Index*



## View

> 사용자 편의와 데이터베이스의 보안 때문에 가상의 테이블이라 할 수 있는 View 를 생성한다. 임의의 View 작성을 통해 매번 복잡한 SQL문을 작성하지 않도록 할 수 있다.



#### View의 종류

|    종류     |                  설명                  |               비고                |
| :---------: | :------------------------------------: | :-------------------------------: |
|   심플 뷰   |  하나의 테이블에서 데이터를 생성한다.  |     CREATE VIEW 명령어로 생성     |
| 컴플렉스 뷰 |         여러개의 테이블을 join         |     CREATE VIEW 명령어로 생성     |
|  인라인 뷰  | SELECT 문의 FROM 절에 기술한 SELECT 문 | 1회용 뷰로 권한을 제어할 수 없다. |

#### 주의할 점

인라인 뷰(Inline View)는 다른 뷰와 특성이 달라 별도 구분하기도 한다.



## Optimizer

> 정해진 우선순위 또는 통계 정보를 이용하여 SELECT 문의 질의 성능이 최적화될 수 있도록 실행계획을 수립한다.



#### 옵티마이저의 작동 원리

<p align='center'><img src = "https://thebook.io/img/006977/227.jpg"/></p>

- `CBO` : Cost Based Optimizer 

- `SBO` : Rule Based Optimizer

  |   구분   |             CBO              |               SBO                |
  | :------: | :--------------------------: | :------------------------------: |
  |   개념   | 사전에 정의된 규칙 기반 계획 |   최소비용 계산, 실행계획 수립   |
  |   기준   |        실행 우선순위         |           액세스 비용            |
  |   성능   |   사용자의 SQL 작성 숙련도   |       옵티마이저 예측 성능       |
  |   특징   |  실행 계획의 예측이 용이함   |     저장된 통계 정보의 활용      |
  | 고려사항 | 저효율, 사용자의 규칙 이해도 | 예측 복잡, 비용 산출 공식 정확성 |

  

## Index

> 인덱스(Index)는 데이터를 찾기 위한 '색인'으로 데이터의 주소록이라고 부를 수 있다. 데이터를 빠르고 효율적으로 조회하기 위해 사용한다.



#### 데이터의 조회 단계(버퍼 캐시에 주목)

<p align='center'><img src = "https://thebook.io/img/006977/230.jpg"/></p>

- `버퍼 캐시` : 자주 사용되는 테이블의 데이터 정보가 저장되어 있는 일종의 '가속장치' 이다.

- 버퍼캐시에 조회하는 데이터가 없다면 우리는 3번과 같이 **데이터베이스** 안에 있는 데이터를 찾아내야 한다.

- 모든 데이터를 버퍼 캐시에 저장할 수는 없다.(관리 효율성 & 비용 문제)

- 내가 원하는 데이터가 어디 존재하는 지 알려주는 **색인** 이 있다면 훨씬 쉽게 데이터를 검색해서 반환할 수 있다.

- 이 색인 값만 별도로 만들어서 관리하는 기법이 **인덱스** (== `Row_id`)

  ![](data:image/jpeg;base64,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)



#### 주의할 점

- 분석시스템(OLAP) & 운영시스템(OLTP)에 따라 인덱스 유형이 달라진다. 
- 인덱스가 지나치게 많으면 과부하 발생

#### 추천 상황

- 열이 WHERE 절의 join 조건으로 자주 사용될 때
- 열에 다양한 값이 있거나 null 값이 많을 때
- *그 반대 상황은 사용하지 않는 편이 낫다.*