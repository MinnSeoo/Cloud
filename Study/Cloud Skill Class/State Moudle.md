# **State Moudle에 대하여**

### **State Moudle 이란 ? ? ?**

인프라스트럭처 자동화 및 구성 관리 도구에서 사용되는 개념으로, 일반적으로 인프라스트럭처(기반 시설) 구성을 정의하고 구성 상태를 유지하는데 사용되는 도구나 기능을 말한다.

<br>

### **State Moudle 의 작업 수행**

- **구성 정의** : 시스템 구성 요소의 상태를 정의하는 코드 또는 파일을 작성한다.

- **상태 확인** : 현재 시스템 상태를 확인하여 정의된 구성과 비교하여 필요한 변경 사항을 식별하고 구성을 일치시킨다.

- **변경 사항 적용** : 필요한 변경 사항을 적용하여 시스템을 원하는 상태로 조정한다. 이는 구성 요소의 설치, 설정 변경 서비스 실행 등을 포함한다.

- **상태 유지** : 시스템 구성이 정의된 대로 유지되도록 주기적으로 확인하고 필요한 변경을 적용시킨다.

<br>

### **AWS에서 지원하는  State Moudle 서비스**
- **CloudFormation** : 인프라스트럭처를 코드로 정의하여 AWS 리소스를 생성, 구성 및 관리 하는 서비스 이다.

- **CDK(Cloud Development Kit)** : AWS 리소스를 프로그래밍 방식으로 정의하기 위한 오픈 소스 개발 도구이다.

- **CLI(Command Line Interface)** : AWS 리소스를 관리하기 위한 도구로, 다양한 AWS 서비스에 대한 명령을 실행하고, 리소스의 상태를 확인 및 구성을 변경할 수 있다.