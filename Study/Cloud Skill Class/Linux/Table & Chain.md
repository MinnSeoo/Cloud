# **Linux Table & Chain**

### **Table & Chain**

리눅스에서** iptables의 RULE 개념 중 하나**이다.

Table 과 Chain에 관련된 내용은 iptables man page에 기술되어 있다.

<br>

### **Table 이란 ?**

Table은 Filter, NAT, Mangle, Raw 이렇게 네 가지로 구분되며, 가각 사전에 정의된 Chain을 가지고 있다. 

Table 은 kernel level 에서 미리 정의되어 추가 및 삭제가 용이하지 않다. 

반면 Chain은 사용자가 임의로 추가하거나 삭제하는 옵션을 가지고 있기 때문에 사용자 정의 rule 이 필요할 경우엔 Table 보단 Chain을 활용하는 것이 좋다. 

- **Filter** - 가장 기본적인 테이블, 패킷 필터링을 담당한다.
- **NAT** - IP와 PORT 등을 변환하는 역할을 한다.
- **Mangle** - 특수 규칙을 이용해 패킷 구조를 변경한다.
- **Raw** - Netfilter API의 기본 tracking 과 별개로 존재하는 룰을 만들때 사용된다.

<br>

### **Chain 이란?**

리눅스에서 iptables에서 사용되는 규칙 세트를 말한다.

각 chain은 패킷을 처리하는 규칙들의 집합으로 이루어져 있으며, **통신의 방향을 결정**한다.

<br>

### **Chain에 대한 작업**

리눅스에서 통신의 방향은 크게 세 가지로 구분된다. 

1. 외부에서 리눅스로 들어오는 INPUT
2. 리눅스에서 외부로 나가는 OUTPUT 
3. 외부에서 리눅스를 경유하여 지나가는 통신에 대한 Chain Forward로 구분할 수 있다.

각 Chain은 리눅스 관리자가 지정한 정책들이 들어가게 되고, 각 Chain은 규칙과 일치하지 않은 패킷을 버릴지, 허용할 지에 대해 **미리 정의**가 되어 있어야 한다. 

<br>

**[ 방화벽 정책 2가지 ]**

1. 모든 규칙을 허용하되, 일부 규칙과 일치하는 통신은 거부한다 (blacklist 방식)
2. 모든 규칙을 불허하되, 일부 규칙과 일치하는 통신만 허용한다. (whitelist 방식)