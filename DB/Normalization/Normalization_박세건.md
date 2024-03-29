
![](https://velog.velcdn.com/images/parksegun/post/8873e6a1-6bcb-4af2-ab03-6ba71deb6754/image.jpg)
# Key
- 무엇을 식별할 수 있는 식별자
- DB에서는 다른 행과 구별할 수 있는 조건이 되는 **속성**의 집합

## Super Key(슈퍼 키)
- 특정 행을 찾아 낼 수 있는 Key(유일성)
  - 유일성 : 특정 행을 바로 찾아낼 수 있는 속성
## Candidate Key(후보 키)
- 특정 행을 찾아 낼 수 있는 **최소한의 Key**(유일성, 최소성)
  - 최소성 : 최소한의 속성의 집합을 갖는 속성
- 유일성과 최소성을 만족해야함
## Primary Key(기본 키)
- 후보키들 중 하나
- 테이블당 1개만 선택 가능
- Null일 수 없고, 중복 불가
## Alternate Key(대체 키)
- 후보키들 중 기본키가 되지 못한 후보키
## Foreign Key(외래 키)
- 다른 테이블의 데이터를 참조하는 키
- 존재하지 않는 값을 참조할 수 없도록 하는 제약설정
- 참조하는 부모의 키는 기본 키이어야 한다
- 외래키가 설정되었다면 부모는 자식보다 먼저 삭제될 수 없다.
# 정규화
- **이상현상**을 방지하기 위한 설계 과정
- 이상현상이 존재하는 테이블을 분리한다.
- 중복을 제거하고 데이터의 일관성과 정확성(데이터 무결성) 유지 가능

>
이상현상(Anomaly) : DB의 일관성이 없거나 잘못된 데이터가 밟생하는 현상
- 삽입 이상 : 의도되지 않은 데이터들이 삽입
- 삭제 이상 : 의도되지 않은 데이터들이 삭제
- 갱신 이상 : 일부만 갱신되는 이상현상

#### 단점 
- Join 연산이 증가하게된다(연관관계가 많아지기 때문)

## 1차 정규화
- 각 컬럼이 원자값(하나의 데이터)를 갖도록 분리
![](https://velog.velcdn.com/images/parksegun/post/c8f61dd7-a972-4c52-8007-df399b22b34f/image.png)
#### 분리 후
![](https://velog.velcdn.com/images/parksegun/post/cf8eb2de-80cf-410a-96e9-ca4609c97868/image.png)


## 2차 정규화
- 1정규화를 거친 후 **부분 함수 종속을 제거**
  - 부분 함수 종속 : 기본키 외에 다른 키에 의해 정해지거나(종속) 기본 키를 구성하는 속성들의 부분집합으로 종속되는 경우
- 완전 함수 종속이 되도록 테이블을 분해
  - 완전 함수 종속 : 오직 기본키에 의해서만 종속
- 중복을 제거
![](https://velog.velcdn.com/images/parksegun/post/444506ba-beac-442b-96e7-cbd5a6c27254/image.png)
#### 분리 후
![](https://velog.velcdn.com/images/parksegun/post/32db1f49-2b98-4ac1-83eb-78ec6bdde134/image.png)

## 3차 정규화
- 2정규화를 거친후 **이행적 종속을 제거**
  - 이행적 종속 : A->B 이고 B->C 이라면 A->C 의 종속을 갖는 것
- 모든 컬럼이 기본키에만 직접적으로 종속되어야함
- ex) 고객번호로 등급을 정하는데 등급이 할인율을 결정한다면 분해
![](https://velog.velcdn.com/images/parksegun/post/b6cb8bf4-5f28-49ae-af79-b4cc409359f5/image.png)
#### 분리 후
![](https://velog.velcdn.com/images/parksegun/post/c9a98a68-e911-4e08-b482-bd0e93538fcd/image.png)

## BCNF(boyce-codd normal form)
- 3정규화를 거친후 *모든 결정자가 후보키가 되도록 분해*
- 특정 컬럼을 결정할 수 있는 결정자가 후보키가 될 수 없는 경우에 분해
ex) 학번, 담당교수 는 후보키로 지정할 수 있는 경우 담당교수 하나의 키가 과목명이라는 컬럼을 결정할 수 있다면 문제 발생 -> 담당교수는 결정자 이지만 후보키가 될 수 없다.
![](https://velog.velcdn.com/images/parksegun/post/529ff193-2bad-45d0-bb9d-a0968e842752/image.png)
#### 분리 후
![](https://velog.velcdn.com/images/parksegun/post/34497ae3-25b5-4023-9204-6308cb9e9fca/image.png)

>
BCNF까지의 정규화를 표준으로 하고있다. 그 이상 정규화를 하면 정규화의 단점이 나타날 수도 있기 때문이다.

[추가적인 정규화 정보](https://code-lab1.tistory.com/270)
## 4차 정규화
### 주요 특징
- 다치 종속 제거
## 5차 정규화
### 주요 특징
- 조인 종속 제거
