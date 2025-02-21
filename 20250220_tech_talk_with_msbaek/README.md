# 백명석님과의 Tech Talk on Feb. 20, 2025

[주 강의 자료](https://github.com/philipjkim/jjamtong/blob/55472d1ddb86d0ff9e25d818f0f9bcd125a7f4d3/20250220_tech_talk_with_msbaek/20250220_tech_talk_by_msbaek.pdf)

## Comments

- actor -> function(procedural(transaction script) <-> OOP(Domain Model))

리팩토링 과정:
- 거대한 함수 리팩토링
  - slide statements
  - chunk statements 
  - comments 
- slap, composed method pattern
- split phase
  - different abstraction level(WEB, APP, Domain, Infra)
  - unreleated complexity(의존 객체)  

Hexagonal 구조의 port in / use case 는 굳이 나누지 않아도 됨. 그러나 port out 과 adapter 는 합칠 수 없음
- adapter
- port in  I / Use Case
- domain
- port out I
- adapter  - 의존성의 방향 때문에 합칠 수 없어 

JPA repository 는 인터페이스처럼 보이지만 실제로는 구현을 포함한 Proxy 임

service(app., domain) -> repository(jpa) unit test 불가. 모든 테스트는 다 @SpringBootTest 
                      -> pure repository interface -> impl -> jpa repository
                                              ^-- mocking, faking(InMemoryRepositoryImpl. hashMap, AtomicLong)

repository : entity = 1 : 1 X
repository : aggregqte = 1 : 1 O
aggreagete : entity : 1 : n(1..2 + value object)
aggregate 간의 참조: 메모리 참조 X, ID 참조(Repository)

average 구하는 함수의 테스트 케이스들:
- null, null
  ```java
  // Bad pattern (too many nests)
  if(!null) { // guard clause (early return)
    if(!0) {
      // ...
    }
  }

  // Better pattern (no or less nests)
  if(null) return;
  if(0) return;

  // ...
  ```
- 0, 0 
- 0, 2 / 0, 4 
- 254, 832 - target design

3 implementations for TDD:
 - fake it till you make it (4)
 - triangulation (4)
 - obvious implementation(5) -> getting stuck -> stair steps test

## 참고할만한 영상/문서들

- [Ian Cooper 의 YT 영상들 (마법수정구 공식보유자)](https://www.youtube.com/results?search_query=ian+cooper)
- [Full-stack ReScript. Architercture Overview](https://fullsteak.dev/posts/fullstack-rescript-architecture-overview)
  - ![sandwich](https://fullsteak.dev/static/0f60a904d74a32001f3adf2879d18f04/acfc1/sandwich.png)
- [Mock Roles, not Objects](http://jmock.org/oopsla2004.pdf)
- [Composed method, SLAP](https://github.com/msbaek/memo/blob/master/refactoring-tech/composed_method.md)
- TDD Process:
  ![TDD process](https://github.com/philipjkim/jjamtong/blob/main/20250220_tech_talk_with_msbaek/TDD_process.png?raw=true)
- Dreyfus model (숙련과정 5단계 모델):
   ![Dreyfus model](https://github.com/philipjkim/jjamtong/blob/main/20250220_tech_talk_with_msbaek/dreyfus_model.png?raw=true)
- LLM 을 이용해 테스트 케이스들을 효율적으로 생산해낼 수 있다. 단 system prompt 에 공을 들여야 이후 질문을 대충 해도 질 높은 답을 얻을 수 있다.
