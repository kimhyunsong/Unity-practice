# Unity Navigation Mesh

- NavMeshAgent : 네비게이션 메시 정보를 바탕으로 이동하는 오브젝트
- OffMeshLink : 연결이 끊어져 있는 절벽, 낭떠러지, 사다리 등을 이동 가능하게 설정
- Navigation Mesh : 게임월드에 이동 가능, 이동 불가능 등의 경로를 데이터로 저장
- NavMeshObstacle : 이동하는 장애물의 네비게이션 메시 정보를 실시간으로 설정





## 1. Navigation View

- Navigation Mesh 설정
- Window - AI - Navigation

### Agent : 네비게이션 메시 정보를 바탕으로 움직이는 에이전트에 대한 설정( NavMeshAgent 컴포넌트)

- AgentType : "+"로 새로운 에이전트 속성을 추가할 수 있다.
- Agent 정보 
  - Name : Agent Type에 보여지는 이름
  - Radius : 에이전트의 반지름
  - Height : 에이전트의 높이(키)
  - Step Height : 오르내릴 수 있는 계단의 높이
  - Max Slope : 올라갈 수 있는 경사 각도

### Areas : 네비게이션 메시로 사용되는 오브젝트들의 구역 설정

Name : 구역 이름으로 기본 Walkable(이동 가능), Not Walkable(이동 불가능), Jump(뛰기)가 제공되고, User 3 부터 원하는 구역을 추가로 설정할 수 있다.

Cost : 구역과 함께 등록. 이동하는데 소요되는 비용 (1 이상). 경로를 탐색할 때 Cost 정보를 기준으로 최단거리를 찾게된다.

Cost 가 2인 Jump는 Cost가 1인 Walkable을 지나갈  때 보다 2배의 시간이 걸린다는 뜻

(실제 오브젝트의 물리적인 이동 속도가 느려지는 것이 아닌 경로를 계산할 때 활용된다.)

### Bake : 네비게이션 메시 데이터를 생성

Baked Agent Size 

- Agent Radius : 에이전트가 지나갈 수 있는 반지름
- Agent Height : 에이전트가 아래로 지나갈 수 있는 높이
- Max Slope : 에이전트가 올라갈 수 있는 경사 각도
- Step Height : 에이전트가 오르내릴 수 있는 계단의 높이

Generated Off Mesh Links

오프 메시 링크는 올라가기 힘든 언덕, 사다리, 절벽 등을 연결해서 이동 가능하게 만드는 옵션이다.

Drop Height : 이동할 수 있는 절벽 아래의 높이

Jump Distance : 뛰어서 넘을 수 있는 절벽 거리



"Bake" 버튼

Navigation에 설정된 옵션들을 바탕으로 네비게이션 정보를 데이터로 굽는다. 





