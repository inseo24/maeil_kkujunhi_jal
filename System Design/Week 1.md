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

1. Spec 정의
    1. 기능적
        1. 짧은 URL alias를 만듦
        2. HTTP 301 Redirect
        3. (optional) Expiration time
        4. (optional) custom alias를 선택
        5. 유저가 삭제할 수 있음
    2. 비기능적
        1. Highly available
        2. latency가 작은 Redirection
        3. 원래의 링크가 추측되선 안됨
2. Capacity Estimation
    1. 합리적인 숫자를 선택
        1. Read:Write = 100:1 
        2. 매달 500M 의 새로운 URL Shortenings
        3. 5년간 URL을 저장
    2. System Capacity 도출
        1. 100 * 500M → 50B redirection
        2. 500M / (30 * 24 * 3600) → 200 URL/s write
        3. 100 * 200 = 20K/s read
        4. 500M * 12 * 5 = 30B URLs
        5. 500 bytes for long + short URL: 30B * 500 bytes = 15TB
        6. 200 * 500 bytes = 100 KB/s incoming data
        7. 20K * 500 bytes = 10 MB/s outgoing data
    3. Cache 추측
        1. 80-20 규칙을 가정
        2. 20K * 3600  * 24 = 1.7B requests / day
        3. 0.2 * 1.7 B * 500 bytes = 170GB
        4. 서버 1개로 캐싱이 충분함
3. System API
    1. createURL (api_key, original_url, custom_alias=None, user_name=None, expire_date=None) → short URL
    2. deleteURL (api_key, url_key)
4. Database Schema
    
    <img width="545" alt="image" src="https://user-images.githubusercontent.com/84627144/210278463-50ab0ad0-933e-499b-b2a7-63f36a724e31.png">
    
    1. Relational DB
    2. Bigtable or DynamoDB 도 괜찮
5. High level systems(아래 사진은 Component Design 후 개선된 시스템)
    
    <img width="582" alt="image" src="https://user-images.githubusercontent.com/84627144/210278468-a929007e-d6e0-41b7-9075-41fa863fc2c2.png">
    
6. Component Design
    1. 어떻게 짧게 할 건지?
        1. base64 인코딩
        2. 6 letters → 64^6 = 68 billion
        3. 8 letters → 64^8 = 281 trillion
    2. Hashing
        1. MD5 → 128-bit hash
        2. 128 / 8 (base64) = 21 letters
        3. 섞어서 6자를 랜덤으로 뽑음
    3. original URL이 같으면 같은 short URL를 리턴(private short URL이 아닌 경우)
        1. 증가하는 시퀀스 넘버를 더할건지
        2. 랜덤한 숫자를 더할건지
        3. user id를 더할건지
    4. Duplication
        1. 6 글자면 서비스가 오래될 수록 중복이 생길 수 있음
        2. 해시 충돌이 없을 때까지 retry할 건지?
    5. Offline key generation
        1. unique key를 미리 생성
        2. 키는 테이블에 저장함
        3. Fast write
    6. 사용된 키인지 어떻게 체크할 건지?
        1. 사용 시 체크되는 추가 컬럼을 두거나
        2. 새로운 키와 사용된 키 2개의 테이블을 둔다
        3. 2개의 테이블을 두는게 더 좋은 점
            1. 사용 체크를 위해 키를 조회하는 작업이 O(LogN)이다.
            2. 키를 단순히 추가하는 건 O(1)이다.
            3. 가장 중요한 건 이렇게 하면 캐싱이 된다는 점
        
        <img width="561" alt="image" src="https://user-images.githubusercontent.com/84627144/210278475-934099d8-231e-4fb9-b270-7d607bc823dd.png">
        
7. Scalability
    1. 로드 밸런서
        1. application 서버에선 아무것도 공유하지 않음
        2. Round-robin LB
    2. DB scale-out
        1. Range partitioning
            1. short url의 첫 번째 단어를 사용함
            2. Letter A → server 1, Letter B → server 2
            3. DB의 unbalanced 가 문제
        2. Hash partitioning
            1. hash % num_server ⇒ server ID
            2. or consistent hashing
    3. Caching
        1. KGS : 사용하지 않는 key를 load and cache
        2. 키는 사용된 키 테이블에 추가됨
        3. KGS에 crash가 생기면 key lose가 발생할 수 있음
        4. application server에서 미리 조회해 캐시함
8. Fault tolerance
    1. application server에서 아무것도 공유하지 않는 게 좋음
        1. health check와 automatic scaling을 사용
    2. KGS는 single point of failure(SPOT)
        1. leader-follower replica를 사용
        2. leader 선출은 Paxos(zookeper) 사용
        3. 단, Paxos는 availability를 보장하진 않음
    3. tradeoff에 대해 꾸준히 고민할 것

</details>