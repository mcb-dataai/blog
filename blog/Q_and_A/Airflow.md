# Airflow

1. 질문: 에어플로우란 무엇인가요?    
답변: 에어플로우는 데이터 파이프라인을 관리하기 위한 오픈소스 플랫폼입니다.
2. 질문: 에어플로우의 기능은 어떤 것이 있나요?    
답변: 데이터 파이프라인 스케줄링, 모니터링, 오류 처리, 리트라이, SLA 지원 등 다양한 기능을 제공합니다.
3. 질문: 에어플로우의 주요 특징은 무엇인가요?    
답변: 에어플로우는 스케줄링, 모니터링, 미들웨어 지원, 플러그인 아키텍처, 대화형 웹 UI 등의 특징을 가지고 있습니다.
4. 질문: 에어플로우에서 DAG는 무엇을 의미하나요?  
답변: DAG는 Directed Acyclic Graph의 약자로, 에어플로우에서 데이터 파이프라인을 구성하는 단위입니다.
5. 질문: 에어플로우에서 Task는 무엇을 의미하나요?  
답변: Task는 DAG를 구성하는 하나의 단위 작업으로, 실행 가능한 실행 단위입니다.
6. 질문: 에어플로우에서 Operator는 무엇인가요?  
답변: Operator는 Task의 구현체로, Task가 실행될 때 수행되는 실제 작업을 정의합니다.
7. 질문: 에어플로우에서 Sensor는 무엇인가요?  
답변: Sensor는 Task가 실행되기 전에 특정 이벤트가 발생할 때까지 대기하는 기능을 수행하는 Operator입니다.
8. 질문: 에어플로우에서 Hook은 무엇인가요?  
답변: Hook은 데이터 파이프라인에서 사용되는 데이터베이스, 클라우드 서비스 등의 리소스와 상호작용하기 위한 인터페이스입니다.
9. 질문: 에어플로우에서 Connection은 무엇인가요?  
답변: Connection은 데이터베이스, 클라우드 서비스 등과 같은 외부 리소스의 연결 정보를 저장하는 객체입니다.
10. 질문: 에어플로우에서 Variable은 무엇인가요?  
답변: Variable은 에어플로우에서 사용되는 변수를 저장하는 객체입니다.
11. 질문: 에어플로우에서 XCom은 무엇인가요?  
답변: XCom은 Task 간에 데이터를 공유하기 위한 메커니즘입니다.
12. 질문: 에어플로우에서 DAG의 작성 방법은 어떻게 되나요?  
답변: DAG는 파이썬 스크립트로 작성하며, DAG 클래스를 상속받아 구현합니다.
13. 질문: 에어플로우에서 Task의 종류는 어떤 것이 있나요?  
답변: BashOperator, PythonOperator, SQLOperator, DummyOperator 등 다양한 Task가 있습니다.
14. 질문: 에어플로우에서 스케줄링을 어떻게 구현하나요?  
답변: 에어플로우는 DAG의 스케줄링을 cron과 유사한 구문으로 구현합니다.
15. 질문: 에어플로우에서 리트라이 기능은 무엇인가요?  
답변: 리트라이 기능은 Task가 실패했을 때 자동으로 재시도하는 기능입니다.
16. 질문: 에어플로우에서 SLA 지원이란 무엇인가요?  
답변: SLA 지원은 Task가 정해진 시간 내에 완료되지 않을 경우 경고를 발생시키는 기능입니다.
17. 질문: 에어플로우에서 모니터링 기능은 어떻게 구현되나요?  
답변: 에어플로우는 다양한 모니터링 도구와 연동하여 데이터 파이프라인의 상태를 모니터링할 수 있습니다.
18. 질문: 에어플로우에서 대화형 웹 UI는 무엇인가요?  
답변: 대화형 웹 UI는 에어플로우의 실행 상태를 시각적으로 확인하고 제어할 수 있는 웹 기반 인터페이스입니다.
19. 질문: 에어플로우에서 DAG의 실행 상태는 어떻게 확인할 수 있나요?  
답변: 에어플로우는 DAG의 실행 상태를 대화형 웹 UI나 CLI(Command Line Interface)로 확인할 수 있습니다.
20. 질문: 에어플로우에서 DAG의 실행 로그는 어떻게 확인할 수 있나요?  
답변: 에어플로우는 DAG의 실행 로그를 로그 파일이나 대화형 웹 UI에서 확인할 수 있습니다.
21. 질문: 에어플로우에서 Task의 실행 결과는 어떻게 확인할 수 있나요?  
답변: 에어플로우는 Task의 실행 결과를 XCom이나 로그 파일을 통해 확인할 수 있습니다.
22. 질문: 에어플로우에서 DAG의 스케줄링을 변경하는 방법은 무엇인가요?  
답변: DAG의 스케줄링을 변경하려면 DAG 파일을 수정하거나, 대화형 웹 UI에서 DAG의 스케줄링 설정을 변경할 수 있습니다.
23. 질문: 에어플로우에서 DAG의 종속성을 변경하는 방법은 무엇인가요?  
답변: DAG의 종속성을 변경하려면 DAG 파일을 수정하거나, 대화형 웹 UI에서 DAG의 종속성 설정을 변경할 수 있습니다.
24. 질문: 에어플로우에서 Task의 실행 순서를 변경하는 방법은 무엇인가요?  
답변: Task의 실행 순서를 변경하려면 DAG 파일을 수정하거나, 대화형 웹 UI에서 Task의 실행 순서를 변경할 수 있습니다.
25. 질문: 에어플로우에서 Task 간에 데이터를 공유하는 방법은 무엇인가요?  
답변: Task 간에 데이터를 공유하려면 XCom을 사용하거나, 외부 데이터베이스나 메시징 시스템을 활용할 수 있습니다.
26. 질문: 에어플로우에서 DAG의 실행을 일시 중지하는 방법은 무엇인가요?  
답변: DAG의 실행을 일시 중지하려면 대화형 웹 UI에서 DAG를 일시 중지하거나, 실행 스케줄을 변경하여 실행을 중지할 수 있습니다.
27. 질문: 에어플로우에서 DAG의 실행을 재개하는 방법은 무엇인가요?  
답변: DAG의 실행을 재개하려면 대화형 웹 UI에서 DAG를 재개하거나, 실행 스케줄을 변경하여 실행을 재개할 수 있습니다.
28. 질문: 에어플로우에서 Task의 실행을 일시 중지하는 방법은 무엇인가요?  
답변: Task의 실행을 일시 중지하려면 대화형 웹 UI에서 Task를 일시 중지하거나, Task의 스케줄을 변경하여 실행을 중지할 수 있습니다.
29. 질문: 에어플로우에서 Task의 실행을 재개하는 방법은 무엇인가요?  
답변: Task의 실행을 재개하려면 대화형 웹 UI에서 Task를 재개하거나, Task의 스케줄을 변경하여 실행을 재개할 수 있습니다.
30. 질문: 에어플로우의 장단점은 무엇인가요?  
답변: 에어플로우의 장점으로는 스케줄링, 모니터링, 리트라이, SLA 지원 등 다양한 기능을 제공하며, 대화형 웹 UI를 통해 쉽게 실행 상태를 확인할 수 있습니다. 단점으로는 초기 설정이 복잡하며, 비교적 높은 리소스 사용량과 성능 저하가 발생할 수 있습니다.