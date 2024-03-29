# **EFS와 EBS에 대하여**

## **EFS**

EFS는 Elastic File System의 약자로 이름에서 알 수 있듯이, AWS의 클라우드 서비스와 온프미스 리소스에서 사용할 수 있는 간단하고 확장 가능하며 **탄력적인 파일 스토리지를 제공**하는 서비스이다.

EFS는 여러 EC2 인스턴스 간에 파일 시스템을 공유 가능하다. 따라서 여러 인스턴스에서 동시에 엑세스 할 수 있으며 수정이 가능하기에 여러 인스턴스가 동시에 작업하는 분산 애플리케이션에 적합하다.

<br>

## **EBS**

EBS는 Elastic Block Store의 약자로 일종의 하드디스크라고 생각하면 된다. 그리고  EBS는 단일 EC2 인스턴스에 연결된 블록스토리지로, 각 EBS 볼륨은 특정 EC2 인스턴스에 마운트되고 해당 인스턴스에 대해 전용으로 사용된다.

- **블록스토리지** - SAN(Strong Area Network) 또는 클라우드 기반 스토리지 환경에 데이터 파일을 저장하는 데 사용되는 기술이다.
    
    [동작과정]
    
    블록 스토리지는 일단 데이터를 블록으로 분활한 후, 해당 블록을 각각 고유 ID를 지닌 개별 조각으로 저장한다.