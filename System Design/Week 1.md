<details>
<summary>알아야 할 개념</summary>
 
- 분산 시스템
    - Logical clock
    - Consensus(Paxos)
    - CP/AP Systems
    - Pub/Sub
- DB Systems
    - (Distributed) Transactions
    - Log-based data structure
    - 2PL vs MVCC
- Practical Knowledge
    - Consistent Hashing
    - Sharding
    - Replication
    - Caching
- Template
    - Spec Identification (5 min)
    - Capacity Estimation (3 min)
    - API Design (3 min)
    - High Level Systems (5 min)
    - Database Schema (5 min)
    - Component Design (10 min)
    - Scalability (5 min)
    - Fault Tolerance (5 min)

</details>


<details>
<summary>템플릿</summary>

1. Spec identification (functional & non-functional) 
    1. Lead but ask a lot
    2. What is the functionality?
    3. Makes sure you & interviewers agreeSimplify (only the core functions)
    4. Non-functional attributes
        1. Availability
        2. Durability
        3. Consistency
        4. Performance and cost
2. Capacity Estimation
    1. reasonable 숫자를 정하기
        1. 1M의 유저인지 1B인지?
    2. QPS, Storage Capacity, Network bandwidth, Number of machines
3. API Design
    1. 핵심 기능을 API로 나타내기
    2. spec을 명확히 하는데 도움
4. High Level Systems
    1. Components를 생각해보기
        1. User(client devices)
        2. Load Balancer
        3. Blob Storage
        4. Database
        5. Compute nodes
        6. Queues
    2. Data flow
        1. b/w components를 그렵기
        2. 해당 flow를 명확히 설명하기
5. Database Schema
    1. metadata를 모델링
    2. table(pk, column, 잘하면 index까지?)
6. Component Design
    1. detail을 채우기
        1. 트위터라면 push일까 pull model일까?
        2. 주키퍼에서의 메타데이터는? relational DB를 사용할지?
        3. 어떻게 global UUID를 생성할지?
7. Scalability & Fault tolerance
    1. Load-balancing
        1. Scale-out
    2. Caching
        1. Cache replacement?
        2. How many cache nodes?
    3. Sharding
        1. Range-Based?
        2. Consistent hashing?
    4. Replication
        1. Consistency?
8. 추가 주제
    1. Operational simplicity
    2. Team
    3. Security
    4. Strategy

</details>


<details>
<summary>URL Shortener를 디자인 한다면?</summary>

</details>