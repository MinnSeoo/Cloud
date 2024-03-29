# **RDS 개념 정리**

## **RDS 란  ? ? ?**
RDS란 AWS 지원하는 서비스 중 하나로 관계형 데이터베이스를 간편하게 클라우드 환경에서 설정, 운영, 확장이 가능하도록 지원하는 웹 서비스이다.

RDS는 mySQL, MarraDB 같은 데이터베이스의 설치, 모니터링, 백업 알람 등 관리를 대신하며, 하드웨어 프로비저닝, 데이터베이스 설정, 패치 및 백업 같이 잦은 운영 작업을 자동화하여 효율적이고 쉽게 크기를 조정할 수 있는 DB 서비스를 제공한다.

EC2 자체가 컴퓨터이기 때문에 EC2 인스턴스에 직접 DB를 설치해서 사용해도 된다.

하지만 AWS RDS를 사용하면 EC2에 직접 DB를 설치하여 운영하는 것보다 더 많은 부분들을 자동으로 관리할 수 있어 편리하기에 많이 애용된다.

<img width="460" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/f2a48a1c-4955-428f-a0ce-e936f4a39a37">

<br>

> 참고 : [https://inpa.tistory.com/entry/AWS-📚-RDS-개념-아키텍쳐-정리-이론편](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-RDS-%EA%B0%9C%EB%85%90-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90-%EC%A0%95%EB%A6%AC-%EC%9D%B4%EB%A1%A0%ED%8E%B8)
> 

<br>

RDS의 DB 제공방식은 EC2와 비슷하다. 

EC2 인스턴스를 생성해서 컴퓨팅을 사용하듯이, RDS 인스턴스를 생성해서 DB를 사용하는 원리이다.

하지만 EC2 인스턴스는 생성후 접속이 가능하지만, RDS는 user가 직접 시스템에 접속하는 것은 불가능하다.

따라서 RDS 인스턴스의 OS패치, 관리 등은 AWS에서 전담 한다.

또한 RDS는 DB의 모니터링 기능을 지원하며, DB에서 발생하는 여러 로그 기록들을 CloudWatch와 연동하여 확인 가능하다.

<br>

## **동작 원리**

RDS는 기본적으로 VPC 안에서 동작하며, Public IP를 부여하지 않아 외부에서 접근이 불가능 하다.

다만, 설정에 따라 Public IP를 open 가능하다. 대신 로드밸러서와 같이 DNS로만 접근이 가능하다.

그리고 VPC 내에서 동작하는 서비스이니 서브넷과 보안그룹도 설정 해 줘야한다.

RDS 인스턴스 생성시 인스턴스 타입을 지정해 줘야하며, 스토리지는 EBS를 그대로 활용하기 때문에
EBS 타입의 유형도 지정해 줘야한다.

그리고 RDS는 유동적으로 데이터 저장소 용량을 증설하는것이 아닌, 생성 시 EBS의 용량을 지정해서 생성한다.
(추후 변경 가능!)

<br>

## **RDS 특징 & 기능**
### **관리 부담 감소**

 RDS를 생성 후 몇 분 이내에 DB 인스턴스를 시작하고 애플리케이션을 연결시킬 수 있으며, DB 파라미터 그룹을    사용하면 DB를 세부적으로 제어하고 튜닝할 수도 있다.

- **튜닝** - db 시스템의 성능을 향상시키기 위해 수행하는 작업을 말함.

 바로 위에서 설명한 특징인 DB 파라미터 그룹은 DB의 설정값들을 모아 그룹화한 개념으로  **RDS의 가장 큰 특징**  이라고 볼 수 있다.

 좀 더 쉽게 말하자면 DB 설정들을 모은 그룹은 각 DB 인스턴스에 적용시켜 DB 설정값을 적용하는 시스템 개념이라고 볼 수 있다.

 **그럼 이 개념을 왜 사용해야 할까 ?  WHY ? ? ?** 

 위에서 설명했듯이 RDS 인스턴스를 직접 수정할 수 없기 때문에 이러한 **우회적인 방법**으로 설정값을 세팅하기 위해서!!

<br>

### **확장성**

 **유연하며 즉각적인 컴퓨팅 규모 조정** : 배포에 사용할 컴퓨팅 및 메모리 리소스를 최대 vCPU 32개와 RAM 244의 범위 내에서 확장 및 축소가 가능하며, 컴퓨팅 규모 조정 작업은 일반적으로 몇 분이면 완료된다.

 **읽기 전용 복제본 시스템 :** RDS는 DB 인스턴스의 복제본을 하나 이상 생성하여 대량의 애플리케이션 읽기 트래픽을 처리할 수 있는 기능을 제공한다. 

<br>

### **가용성 및 내구성**

 **자동 백업** : RDS는 DB와 트랜잭션 로그를 백업하고 사용자가 지정한 보존 기간 동안 이를 모두 저장할 수 있는 기능을 지원한다. 이를 통해 DB 보존 기간 중 어느 시점(초 단위)으로 복원할 수 있다.

 **DB 스냅샷** : 사용자는 원하는 경우 언제든 DB 스냅샷을 통해 새 RDB 인스턴스를 생성할 수있다.

 **자동 호스트 교체** : 하드웨어에 장애가 발생할 경우, 배포를 지원하는 컴퓨팅 인스턴스를 자동으로 교체하기 때문에 장애 복구 시간이 빨라진다.

 위에서 설명한 RDS의 특징 중 하나인 백업 시스템에 대해서 좀더 알고 싶어져서 RDS의 백업 시스템에 대하여 좀 더 찾아보았다.

<br>

### **RDS 백업 시스템**

---

보통 백업 작업은 **Data Export**를 통해 **DB dump**파일로 저장하여 보관한다. 하지만 RDS는 위에서 설명했듯이
특정 시점의 **스냅샷**을 저장하여 언제든 복원 가능하며 자동 백업 기능을 활성화 하기 때문에 매우 편리하다.

- **dump 파일** : DB, 메모리, 파일 시스템 등의 정보를 백업하거나 저장하기 위해 사용되는 이진 형식의 파일.

<br>

**[ RDS 백업 설정 ]**

AWS RDS는 설정에 따라 자동으로 백업을 생성한다. 기본적으로 매일 한 번의 백업이 생성되며, 백업 시간은 사용자가 직접 설정 가능하다. 그리고 자동 백업은 스냅샷 형태로 AWS S3에 저장된다. 

또한 백업 스냅샷의 보존 기간은 기본적으로 7일이다.

<br>

**[ 자동 스냅샷 생성 ]**

AWS RDS는 Default 설정으로 하루에 한 개씩 스냅샷을 자동으로 생성한다. 이 스냅샷을 통해 데이터를 복구하면 기존 RDS 인스턴스의 데이터가 복구되는 것이 아닌 복귀 시점의 데이터를 바탕으로 새로운 RDS 인스턴스를 생성하는 것이다.

<br>

**[ 수동 스냅샷 생성 ]**

사용자가 필요한 때에 수동으로 스냅샷을 생성할 수 있다. 예를 들면 중요한 변경 전에 백업을 수행하거나 테스트 목적으로 데이터를 백업하는데 사용된다.

<br>

**그럼 자동과 수동의 차이는 ? ? ?**

→ 기존의 RDS 인스턴스르 삭제하면 **자동으로** 백업된 스냅샷들을 **같이 삭제**되지만, **수동**으로 백업한 스냅샷은 **그대로 유지**가 된다.

그렇기 때문에 혹시나 모를 상황에 대비하여 RDS 인스턴스를 삭제하기 전에 수동으로 스냅샷을 저장하고 삭제하는 것이 좋을것 같다.