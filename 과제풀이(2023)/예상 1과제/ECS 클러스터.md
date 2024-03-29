# **예상 1과제 풀이 (5. ECS 클러스터)**

> ### **[ECS 클러스터 문제]**

> ### a. webapp이라는 이름으로 ECS 클러스터를 생성하세요.

> ### b. 해당 ECS 클러스터에서 생성되는 task는 위에서 생성한 VPC에 있는 프라이빗 서브넷에서 실행되어야 합니다.

> ### c. 오토스케일링 그룹을 통해서 용량공급자를 추가하고 용량공급자를 통해서 생성되는 ECS 인스턴스의 최소 갯수는 0개에서 최대 5개까지 늘어날수 있어야 합니다.

> ### d. ECS 인스턴스의 AMI는 Amazon Linux를 사용해야 하고 인스턴스 타입은 t3.medium으로 합니다.

> ### e. SSH를 통해서 해당 ECS 인스턴스에 접속할 필요는 없습니다.

<br>
<br>

**[풀이 🔽]**

- **a**에서 클러스터를 생성하라고 했으므로 검색창에 ECS 검색 후 클러스터 생성 클릭

- **a**에서 클러스터 이름을 webapp이라고 지정하라고 했으므로 이름 →  **webapp**으로 지정

- 네트워킹에서 VPC와 Subnet을 지정 해 준다. 이때 VPC는 기존에 만들어둔 **ws-vpc**를 선택하고 **b**에서 **프라이빗 서브넷**을 선택하라고 했으므로 **prviate subnet (a,b,c)**를 선택해 준다.

- **인프라**는 **Amazon EC2 인스턴스**를 선택
- **d** 에서 인스턴스 AMI는 Amazon Linux를 사용하라고 했으므로 아키텍처는  **Amazon Linux** 선택
- **d** 에서 인스턴스 타입을 t3.medium으로 지정하라고 했으므로 **t3.medium**으로 지정
- **c** 에서 용량이 0개~ 최대 5개 까지 늘어날 수 있다고 했으므로 **최소: 0, 최대: 5** 로 지정
- **e** 에서 ECS 인스턴스에 접속할 필요가 없다고 했으므로 **키 페어는 지정 X**