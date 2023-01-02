<details>
<summary>확장성(키워드 : 수평, 수직, 개선 방안)</summary>

- 확장성 : 증가하는 트래픽을 처리할 수 있는 시스템의 능력
    - Horizontal Scaling
        - 분산 시스템에서 더 많은 시스템을 추가해 트래픽이나 데이터를 처리함
        - 장점 : 수요에 맞게 쉽게 시스템을 확장 또는 축소할 수 있음(유연하고 확장성이 좋음)
            - 갑자기 트래픽이 급증해도 기존 서비스 중단 없이 새 서버를 시스템에 추가해 downtime이 없음
        - 단점 : 수직적 확장보다 구현이나 관리가 더 복잡해짐
    - Vertical Scaling
        - 단일 머신에 더 많은 리소스를 추가해 capacity를 늘림(메모리, CPU, 스토리지)
        - 장점 : 시스템 재구성 없이 빠르게 수행 가능
        - 단점 : 확장에 결국 제한이 있음. 어느 시점부턴 수평 확장이 더 비용상 효율적일 수 있음
- 확장성 개선 방안
    - Load Balancing
        - 로드밸런싱을 이용해 단일 리소스에 과부하를 주지 않고 더 많은 트래픽 요청을 처리한다.
        - 응답성을 유지하면서 수요 변화에 따라 확장이나 축소할 수 있다.
        - Round Robin, least connections, least response time 등 여러 방식이 있음
    - Stateless
        - 이전의 interactions에 대한 state 정보를 유지하지 않도록 시스템을 설계한다.
        - 서버는 각 요청을 독립적인 트랜잭션으로 처리할 수 있어 여러 클라이언트 요청을 동시에 처리할 수 있다.

</details>
 
 <details>
 <summary> Failovers Strtegies</summary>
    
- cold standby(수평/수직 확장에 해당하지 않음)
    - 장애 발생 시 백업 시스템을 포함 + active backup 시스템은 없음
    - 장점 : 간단하고 비용이 저렴함(내부 툴이라면 괜찮을지도?)
    - 단점 : 장애 처리에 시간이 더 걸림, 시스템 복원 시점까지 데이터 손실 발생 가능
- warm standby
    - 장애 발생시 자동으로 원래 작업을 이어 받는 백업 시스템을 포함한다.(active backup only)
    - replication O ← 보통 DB에 내장된 메커니즘이 있어 ON/OFF 해서 쓰면 된다.
        - DB가 down되면 warm-standby로 리다이렉션 된다.
    - 장점 : 더 빠른 장애 처리가 가능
    - 단점 : 리소스 사용량이 더 많음
- hot standby
    - 실행 중인 백업 시스템이 있고 + 기본 시스템의 작업을 이어 받는 백업 시스템까지 포함(both)
    - 실제 모든 데이터를 동시에 모든 DB 인스턴스에 쓴다.
    - 지속적인 WAL archiving(로그 전달log shipping)이나 streaming replication을 함께 사용해 설정할 수도 있다.
    - 장점 : 가장 빠르게 장애 처리
    - 단점 : 가장 많이 리소스가 필요함

 </details>