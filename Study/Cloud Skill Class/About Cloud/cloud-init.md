# **cloud init에 대하여 간략하게 정리**

## cloud-init이란?

다양한 클라우드 플랫폼에서 인스턴스를 초기화 하기 위한 스크립트로 다중 배포 방법을 제공하며, 모든 주요 퍼블릭/ 프라이빗 클라우드 제공업체의 프로비저닝 시스템 및 베어메탈 설치를 지원한다.

OS가 부팅될때 제공된 메타데이터를 읽고 그에 따라 시스템을 초기화 할 수 있다.

ex) ssh 엑세스 키, 네트워크, 스토리지 설정 …등이 있다.

- 프로비저닝 - 사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요 시 시스템을 즉시 사용할 수 있는 상태로 미리 준비해 두는것.

- 베어메탈 - 단일 주소 공간 운영 체제로 최소한의 설치 공간으로 고성능 컴퓨팅을 달성하기 위해 어셈블리어로 작성되었다.

<br>

## Cloud-init이 발생하는 시점

cloud init은 보통 boot 시점에 발생하며, 5가지 단계를 거친다.

- Generator - cloud init을 enable 시켜주는 역할
- Local - 로컬 데이터소스(floppy, iso image)를 사용할때 쓴다
- Network
- Config
- Final