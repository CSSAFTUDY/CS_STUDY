![](https://velog.velcdn.com/images/parksegun/post/4289a5e2-66af-4597-b5e6-662102ea13c5/image.png)
# N+1 문제란?
>
하나의 조회쿼리로 N개의 객체가 조회되는 경우에, 각각의 객체안에 또다른 객체가 존재할 수 있다. 이때 또다른 객체를 조회하기위해서 다시 하나의 조회쿼리가 필요하다. 즉, 하나의 조회쿼리로 N개의 객체가 조회되었고 다시 N개의 내부 객체를 조회하기위해서 N개의 조회쿼리가 필요하기 때문에 처음에 호출했던 1번의 조회와 N번의 조회가 필요한 현상 **N+1 문제**라고 합니다.
=> 많은 SQL 호출은 성능을 악화시킵니다.
![](https://velog.velcdn.com/images/parksegun/post/cf2ac81d-7d7b-48e1-bf03-1ed87e6f61ed/image.png)

# 해결 방법
## Fetch Join 사용
- 가장 일반적인 방법입니다.
- 조회하려는 개체와 연관된 모든 정보를 하나의 쿼리로 가져옵니다.
  - Eager Loading과 Lazy Loading의 단점을 줄이기 위해 등장
  - 조인 쿼리에 fetch를 추가해준다면 글로벌 로딩 전략이 Lazy Loading 이어도 모든 정보를 즉시(Eager Loading)가져온다.
  - 때문에 글로벌 로딩 전략은 Lazy Loading으로 설정한 후에 객체에 대한 모든 정보가 필요할때에는 fetch 조인을 사용해서 즉시 로딩 시켜주는게 효율적이다. 
  
### 그렇다면 join과 다른점은?
- 일반 Join
  - 연관 Entity에 Join을 걸어주어도 실제 select 하는 Entity는 조회하는 주체에 대해서만 조회하고 영속화를 진행합니다. 즉, Join을 걸어주었던 Entity는 select하지 않습니다.
  - 때문에 연관 Entity가 검색 조건에는 필요하지만 데이터 자체는 필요하지 않은 경우에 사용합니다.
- Fetch Join
  - 연관된 Entity들을 모두 select 해서 영속화 시킵니다.
  - 때문에 FetchType이 LazyType이어도 모두 select 하기에 N+1문제를 해결할 수 있습니다.
### 단점
- 두개 이상의 1:N 관계에서 사용할 수 없다(에러 발생).
- 별칭을 제공할 수 없다.
## Batch Size 설정
>Batch Size란, 지연 로딩을 사용하지만 연관된 Entity의 정보가 사용된다면, 설정된 size 만큼의 연관된 Entity를 한번의 쿼리로 가져옵니다.
이 개수를 넘어가는 쿼리가 발생될경우 프록시 객체로 받아오기에 조회할때에 Batch Size 만큼 다시 한번에 가져옵니다.
---
예를 들어서, member Entity가 Team Entitiy와 연관되어있을때 Member를 조회하면서 Team_id 값을 확보합니다. 그 후에 Team 정보가 필요하다면 확보했던 Team_id 값으로 설정된 Batch Size 만큼 Team Entity를 한번의 쿼리로 가져옵니다.

- N+1 문제를 해결하는 것이 아닌 완화할 수 있는 방법이다.
[Batch관련](https://velog.io/@joonghyun/SpringBoot-JPA-JPA-Batch-Size%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0)

## 추가 설명
- JPQL(Java Persistence Query Language) : 
  - JPA에서 제공하는 **객체 지향 쿼리 언어** 입니다.
  - SQL을 추상화한 언어입니다.
  - 문법은 SQL과 유사하지만 테이블명을 사용하는 것이 아닌 **엔티티**(객체)이름을 대상으로 사용합니다.
  - 사용예시
```
TypedQuery<Member> query1 = em.createQuery("select m from Member m", Member.class);
List<Member> resultList = query1.getResultList();
for (Member m : resultList) {
    System.out.println("m = " + m.getUsername());
}
```
- [추가 내용](https://ittrue.tistory.com/270)
- 즉시 로딩(Eager Loading) :
  - SQL 쿼리로 객체를 조회할때에 하나의 쿼리로 조회하고자 하는 객체의 모든 연관관계의 데이터(모든 정보)를 가져온다.
  - 하지만 글로벌 로딩 전략(default)값을 Eager Loading으로 설정한다면 필요하지 않은 정보를 모두 가져오기 때문에 비효율 적일 수 있다.
  - !!Eager Loading과 JPQL을 같이 사용하게 되면 N+1 문제가 발생할 수도 있다!!
  >
  JPQL은 글로벌 로딩 전략을 고려하지 않기때문에 조인을 사용하지 않고 조회하려는 객체의 정보를 가져오게된다(연관관계를 가져오지 못함) 하지만 이때, 글로벌 로딩 전략이 Eager Loading이라면 즉시 연관관계를 가져와야 하기때문에 다시 쿼리를 날리게된다(N+1 발생)
  **이러한 문제 또한 fetch join으로 방지할 수 있다**
- 지연 로딩(Lazy Loading) : 
  - SQL 쿼리로 객체를 조회할때에 조회하려고 했던 객체의 정보 중에서 연관된 정보(객체안에 포함된 다른 객체)를 제외한 오직 객체가 갖고 있는데이터만 조회한다.
_조회하지 않았던 정보들은 일단 프록시 객체로 대체한다_
  그 후에 조회하지 않았던 정보들을 사용해야할 경우가 생긴다면 그때 다시 쿼리를 날려서 데이터를 조회한다(N+1 문제 발생)
  - 필요하지 않은 정보를 가져오지 않을 수도 있다는 장점이 있지만 추후에 사용하게된다면 다시 쿼리를 날리는 비효율적인 현상이 발생한다.
  >예를 들어서 Member는 Team의 정보를 갖고있을때에 Member를 조회하게된다면 Team은 프록시 객체로 대체되고 추후에 Member.team... 과 같은 정보를 조회하게된다면 쿼리가 전송된다.
