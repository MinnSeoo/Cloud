### **VPC란 무엇인가 ? ? ?**

Virtual Priavte Network Computing의 약자로 IP대역(CIDR) 할당하여 가상의 네트워크 환경을 구성하는 것이다.

VPC 유형에도 2가지가 있는데 바로 **VPC Peering**과 **Transit Gateway**이다.

<br>

### **VPC Preeing**

Private IPv4 주소 또는 IPv6 주소를 사용하여 두 VPC 간에 트래픽을 라우팅 할 수 있도록 하기 위한 두 VPC 사이의 네트워킹 연결이다.

<br>

### **Transit Gateway**

user가 자신의 VPC와 온프레미스 네트워크를 단일 게이트웨이에 연결할 수 있도록 지원해 주는 서비스이다.

(이 경우에는 aws VPN을 각 VPC에 따로 연결해야 한다. → 구축하는 데 오랜시간이 걸리며 VPC 수가 많아질 수록 관리하기가 힘들어질 수도 있음.)

<br>

### **Preeing & Transit Gateway**

이 둘은 서로 VPC를 연결한다는 공통점이 존재하지만 VPC를 연결시에 생기는 구조에 차이가 있다.

<br>

**[Peering.img]**

![image](https://github.com/MinnSeoo/Cloud/assets/102645965/86e9fda6-88ee-4e52-a720-d9a5e9fe6817)

Peering의 경우 VPC와 VPC 즉 2개의 VPC 끼리만을 연결하기 때문에 여러 VPC를 사용하게 되면 그에 따라 VPC Peering의 개수가 증가하게 된다. 또, 온프레미스를 모든 VPC에 연결하기 위해서는 모든 VPC VPN Connection을 연결해 줘야한다는 번거러움이 있다.

<br>

**[Transit Gateway.img]**

![image](https://github.com/MinnSeoo/Cloud/assets/102645965/da033fa0-d7b4-48ba-82f7-29f133977a56)

Transit Gateway는 Peering과는 다르게 VPC를 한번에 연결할 수 있게 한다. 또한 Trainsit Gateway에  VPC Connetion을 연결시켜 줌으로서 Connection의 개수가 줄어들수 있다.

이러한 특성을 이용하여 VPC의 구조를 더 단순한 형태로 만들수 있다.