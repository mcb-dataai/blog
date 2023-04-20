# Kafka

1. 카프카는 무엇인가요?    
답변 : 카프카는 분산형 스트리밍 플랫폼입니다.
2. 카프카의 핵심 개념은 무엇인가요?    
답변 : Producer, Consumer, Topic, Partition, Broker
3. 카프카의 주요 용도는 무엇인가요?    
답변 : 대용량의 실시간 데이터 스트리밍 처리
4. 카프카의 사용 사례는 어떤 것이 있나요?    
답변 : IoT 데이터 수집, 로그 수집, 실시간 분석 등
5. 카프카와 JMS의 차이점은 무엇인가요?    
답변 : 카프카는 pub-sub 모델에 최적화되어 있고, JMS는 point-to-point 및 pub-sub 모델 모두 지원합니다.
6. 카프카의 데이터 보존 정책은 무엇인가요?    
답변 : 카프카는 데이터를 보존하기 위해 시간 기반의 로그 롤링 방식을 사용합니다.
7. 카프카의 클러스터 구성은 어떻게 이루어지나요?  
답변 : 카프카 클러스터는 여러 개의 브로커로 구성됩니다.
8. 카프카에서 메시지는 어떻게 전송되나요?  
답변 : 메시지는 프로듀서가 브로커로 보내고, 브로커에서 컨슈머로 전송됩니다.
9. 카프카의 Replication Factor는 무엇인가요?  
답변 : Replication Factor는 각 파티션을 복제할 브로커의 수를 나타냅니다.
10. 카프카에서 Producer는 어떤 역할을 하나요?  
답변 : Producer는 메시지를 생성하고 브로커로 전송합니다.
11. 카프카에서 Consumer는 어떤 역할을 하나요?  
답변 : Consumer는 브로커에서 메시지를 가져와 처리합니다.
12. 카프카에서 Topic은 무엇인가요?  
답변 : Topic은 메시지가 전송되는 대상을 나타냅니다.
13. 카프카에서 Partition은 무엇인가요?  
답변 : Partition은 Topic을 분할한 뒤 분산해서 저장하는 단위입니다.
14. 카프카에서 Offset은 무엇인가요?  
답변 : Offset은 파티션 내에서 메시지를 식별하는 고유한 값입니다.
15. 카프카에서 Leader와 Follower Broker의 차이는 무엇인가요?  
답변 : Leader Broker는 파티션의 쓰기 및 읽기 작업을 담당하고, Follower Broker는Leader Broker의 데이터를 복제하여 백업 역할을 수행합니다.
16. 카프카에서 ISR은 무엇인가요?  
답변 : ISR은 In-Sync Replica의 약자로, 현재 Leader와 동기화된 복제본을 나타냅니다.
17. 카프카에서 Consumer Group은 무엇인가요?  
답변 : Consumer Group은 하나의 Topic에 대해 여러 Consumer가 메시지를 분산 처리하는 데 사용됩니다.
18. 카프카에서 Consumer Group의 동작 방식은 어떻게 되나요?  
답변 : Consumer Group의 Consumer들은 Topic의 파티션을 분배받아 독립적으로 메시지를 처리합니다.
19. 카프카에서 Consumer Offset은 무엇인가요?  
답변 : Consumer가 Topic의 메시지를 어디까지 처리했는지를 나타내는 값입니다.
20. 카프카에서 Commit Offset은 무엇인가요?  
답변 : Consumer가 처리한 메시지의 마지막 Offset을 Broker에게 알리는 것을 Commit Offset이라고 합니다.
21. 카프카에서 Auto Commit과 Manual Commit의 차이점은 무엇인가요?  
답변 : Auto Commit은 일정 주기마다 자동으로 Commit Offset을 수행하고, Manual Commit은 Consumer가 명시적으로 Commit Offset을 수행합니다.
22. 카프카에서 Consumer Rebalance는 무엇인가요?  
답변 : Consumer Group 내의 Consumer 수가 변경되거나 Consumer가 새로운 Topic을 구독할 때, 파티션 분배를 재조정하는 작업입니다.
23. 카프카에서 Producer의 Acknowledgement 모드는 어떤 것이 있나요?  
답변 : Acknowledgement 모드에는 0, 1, all 세 가지가 있습니다.
24. 카프카에서 Message Retention은 무엇인가요?  
답변 : Message Retention은 메시지를 얼마나 오래 보존할 것인지를 설정하는 기능입니다.
25. 카프카에서 Compaction은 무엇인가요?  
답변 : Compaction은 중복 메시지를 제거하고 가장 최근의 메시지만 보존하는 기능입니다.
26. 카프카에서 MirrorMaker는 무엇인가요?  
답변 : MirrorMaker는 다른 클러스터에서 데이터를 복제할 때 사용하는 도구입니다.
27. 카프카에서 Connect는 무엇인가요?  
답변 : Connect는 외부 시스템과 카프카 간에 데이터를 이동시키기 위한 프레임워크입니다.
28. 카프카에서 KSQL은 무엇인가요?  
답변 : KSQL은 SQL 쿼리를 사용하여 스트림 데이터를 처리하는 엔진입니다.
29. 카프카에서 Schema Registry는 무엇인가요?  
답변 : Schema Registry는 Avro 스키마를 관리하고 검증하는 기능을 제공합니다.

30. 카프카의 주요 장점은 무엇인가요?  
답변 : 대용량의 실시간 데이터 처리, 확장성, 고가용성, 높은 처리량, 다양한 툴과 라이브러리 지원 등이 있습니다.