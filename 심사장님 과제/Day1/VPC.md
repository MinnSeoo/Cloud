# DAY 1 - VPC

```
[ 과제 내용 ]
1. Internet G/W, Egress-only internet G/W, NAT G/W의 개념과 차이를 정리하세요.
	 VPC peering의 개념에 대해 정리하세요.

2. 두 개의 VPC를 peering으로 연결할 때 다른 VPC의 DNS private zone의 주소를 쿼리 할 수 있는 
   방법에 대해 조사하고 직접 구성해 보세요.

3. DX 사용 시 하나의 DX G/W 당 연결 가능한 Virtual private G/W는 hard limit으로 인해
   20개 이상 직접 추가를 할 수 없습니다. 20개 이상 연결을 추가할 수 있는 다른 방법을 생각해 보세요.

4. VPC Endpoint와 Private Link에 대해서 정리하세요. 
```

## **[ 1 ]**

### **Internet G/W 란 ? ? ?**

igw는 VPC와 인터넷 간 통신할 수 있게 해준다. 또한 인터넷 라우팅 가능 트래픽에 대한 vpc rtb에 대상을 제공하며, public IPv4 주소가 할당된 인스턴스에 대해 NAT로 변환시키는 두 가지 목적이 있다.

(+ IPv4 IPv6 트래픽을 지원함.)

### **필요한 이유는 ? ? ?**

igw는 subnet 내 ec2와 같은 리소스를 Internet과 연결하기 위해 꼭 필요한 존재이다.

( 참고로 igw는 수평적 확장성과 높은 고가용성을 지니기 때문에 네트워크 대역폭에 제약이 없다.)

### **구체적으로 어떤 역할을 수행 ? ? ?**

좀 더 구체적으로 말하자면 igw는 Public ip / Elastic ip를 가진 인스턴스들에 대하여 NAT로 변환하는 역할을 담당한다.

외부 인스턴스가 Public ip / Elastic ip를 가진 AWS 리소스에 대하여 요청을 보냈을 경우엔 igw는 ip 주소를 변환하여 리소스 내부 ip 주소로 해당 요청을 보내고, 서브넷 내부에 있는 리소스가 vpc 밖에 있는 서비스에 요청을 보낼땐 응답 주소는 해당 Public ip / Elastic ip로 셋팅된다.

따라서 서브넷 router에 연결된 igw 존재하면 해당 서브넷 내에 호스팅되고 있는 리소스는 인터넷 연결이 가능한 것이다. 그리고 이러한 형태의 Subnet을 Public Subnet이라고 한다. (모든 subnet은 default로 Public이다.)

그리고 igw가 Subnet에 연결되지 않은 경우에는 Private Subnet이라고 한다. 

(해당 subnet 내 호스팅되는 리소스는 internet에 접근할 수 없기 때문.)

---

### **Egress-only internet G/W 란 ? ? ?**

서브넷의 인스턴스가 인터넷에 엑섹스 할 수 있도록 허용하지만 인터넷 리소스가 인스턴스와의 통신을 시작하지 못하도록 하는 igw 유형이다. (IPv6 트래픽 만 해당함.)

Private Subnet에 존재하는 IPv6 유형의 인스턴스만 Egress-only internet gateways를 사용하여 외부 인터넷과 연결할 수 있지만 그에 대한 응답은 받지 못한다.

---

### **NAT G/W 란 ? ? ?**

AWS에서 제공하는 NAT 서비스로 Private subnet의 인스턴스가 VPC 외부 서비스(인터넷)에 연결할 수 있도록 하고, 외부에선 Private subnet 내의 인스턴스와 연결할 수 없도록 한다.

즉, Private Subnet 내의 ec2를 AWS Service에 접근 가능하게 하고, 외부에서는 해당 ec2에 대한 접근을 막기위해 사용한다.

---

### 차이점

IGW와 Egress-only IGW의 차이점은 바로 request에 대한 응답을 받을 수 있냐, 없냐의 차이가 있고, 

IGW는 IPv4와 IPv6 두 개의 트래픽을 처리할 수 있지만 Egress-only IGW는 IPv6의 트래픽만 처리할 수 있다는 차이가 있다.

---

### **VPC Peering 이란?**

다른 VPC간에 Private IP로 통신가능하도록 연결하는 것을 말한다.

(참고로 다른 region간의 VPC및 다른 AWS 계정의 VPC와도 연결이 가능하다.)

### Why ???

보통의 VPC간의 통신은 Public IP를 통해 이루어진다. 하지만 Public IP를 통해 통신할 경우에는 데이터의 안정성에 대한 문제가 발생할 수 있다는 단점이 있다. 이를 해결하기 위해 Private  IP 통해 다른 VPC간 통신하기 때문에 데이터의 안정성을 보장받을 수 있기 때문에 VPC Peering을 사용한다.

