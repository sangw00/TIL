# NL Join VS Hash Join

### Nested Loop Join

![SQL_250](https://user-images.githubusercontent.com/66261552/119997185-6d3f4100-c00a-11eb-81fa-2f3556b89f03.jpg))

- **원리**
  - NL 조인은 중첩 루프문과 같은 수행 구조를 사용한다.
  - NL 조인은 Outer와 Inner 양쪽 테이블 모두 인덱스를 이용한다. 
  - Outer 쪽 테이블은 사이즈가 크지 않으면 인덱스를 이용하지 않을 수 있다. 
    - Table Full Scan 하더라도 한 번에 그치기 때문이다.
  - Inner 쪽 테이블은 인덱스를 사용해야 한다.
    - 인덱스를 이용하지 않으면 Outer 루프에서 읽은 건수만큼 Table Full Scan을 반복하기 때문이다.
  - NL 조인은 `인덱스를 이용한 조인 방식`이라고 할 수 있다.
- **특징**
  1. 랜덤 엑세스 위주의 조인 방식이다.
     - 레코드 하나를 읽으려고 블록을 통째로 읽는 랜덤 엑세스 방식은 설령 메모리 버퍼에서 빠르게 읽더라도 비효율이 존재한다. 
     - 인덱스 구성이 아무리 완벽해도 대량 데이터 조인할 때 NL 조인이 불리한 이유
  2. 조인을 한 레코드씩 순차적으로 진행한다.
     - 이 특징 때문에 부분범위 처리가 가능한 상황에서 아무리 큰 테이블을 조인하더라도 매우 빠른 응답속도를 낼 수 있다. 
- NL 조인은 소량 데이터를 주로 처리하거나 부분범위 처리가 가능한 온라인 트랜잭션 처리 시스템에 적합한 조인 방식이라고 할 수 있다.

---

### Hash Join

![SQL_252](https://user-images.githubusercontent.com/66261552/119997090-513b9f80-c00a-11eb-90a2-984d0a6002b4.jpg)

- **원리**
  - 해시 조인은 두 단계로 진행된다.
    1. Build 단계 : 작은 쪽 테이블(Build Input)을 읽어 해시 테이블(해시 맵)을 생성한다.
    2. Probe 단계 : 큰 쪽 테이블(Probe Input)을 읽어 해시 테이블을 탐색하면서 조인한다.
- **특징**
  1. NL 조인처럼 조인 과정에서 발생하는 랜덤 엑세스 부하가 없다.
  2. 인메모리 해시 조인일 때 가장 효과적이다.
  3. 대량 데이터 조인할 때는 일반적으로 가장 빠르다

---

### 조인 메소드 선택 기준

- **일반적인 선택 기준**
  - 소량 데이터 조인 -> NL 조인
  - 대량 데이터 조인 -> 해시 조인
    - 대량의 기준은 NL 조인 기준으로 최적화했는데도 랜덤 엑세스가 많아 만족할만한 성능을 낼 수 없다면, 대량 데이터 조인에 해당한다.
- **수행빈도가 매우 높은 쿼리 기준**
  - (최적화된) NL 조인과 해시 조인 성능이 같을 때 -> NL 조인
  - 해시 조인이 약간 더 빠를 때 -> NL 조인
  - NL 조인보다 해시 조인이 매우 빠를 때 -> 해시 조인
- **NL 조인을 가장 먼저 고려해야 하는 이유**
  - NL 조인에 사용하는 인덱스는 영구적으로 유지하면서 다양한 쿼리를 위해 공유 및 재사용하는 자료구조다.
  - 해시 테이블은 단 하나의 쿼리를 위해 생성하고 조인이 끝나면 곧바로 소멸하는 자료구조다.
  - 같은 쿼리를 100개 프로세스가 동시에 수행하면, 해시 테이블도 100개가 만들어진다. 따라서 수행시간이 짧으면서 수행빈도가 매우 높은 쿼리를 해시 조인으로 처리하면 CPU와 메모리 사용률이 크게 증가한다.
- **해시 조인을 사용할 때**
  - 수행 빈도가 낮고 쿼리 수행시간이 오래 걸리는 대량 데이터 조인할 때
