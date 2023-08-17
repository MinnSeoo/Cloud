## **AppSync 란 ? ? ?**

AppSync는 AWS에서 제공하는 관리형 서비스로, 애플리케이션의 데이터를 효율적으로 모델링하고 API를 생성하며, 다양한 데이터 소스와 상호 작용하도록 도와주는 플랫폼이다.

주로 모바일 앱 및 웹 애플리케이션에 사용되며, GraphQL을 기반으로 하여 데이터 요청 및 응답을 관리하고 데이터 소스와의 통합을 단순화하는데 중점을 둔 서비스이다.

AppSync는 Client에게 하나의 **GraphQL endpoin를 제공**하고, Client로 부터 해당 endpoint로의 request(query, mutaion)를 받으면 query(또는 mutaion)에 대해 등록된 **resolver를 호출**한다.

이때 resolver는 다양한 데이터 소스로부터 데이터를 가져와 **Client에게 response** 한다.

- **mutaion** : 데이터를 변경한 후 가져오기 위한 메서드

### **GraphQL 이란 ? ? ?**
---
페이스북에서 개발한 데이터 쿼리 언어 및 런타임 환경으로, Client와 서버 간의 데이터 통신을 위한 방식을 제공한다. GraphQL은 REST API의 한계와 문제점을 해결하고, 데이터 요청과 응답의 유연성과 효율성을 높히기 위해 설계되었다.

GraphQL은 Client에게 자신에게 필요한 데이터만을 query할 수 있도록한다. 이를 위해 GraphQL은 Client에서 사용할 Query Language를 정의하여 Client가 자신에게 필요한 데이터에 대한 Query를 선언하여 GraphQL에 넘기면 GraphQL은 Query를 해석해 서버에 필요한 데이터를 가저온 후 Client에게 해당 쿼리에 대한 데이터를 반환한다.

### **AppSync Resolver**
---
AppSync는 request를 받으면 데이터소스에 해당 request를 연결하기 위해 resolver를 호출한다.

AppSync는 2 종류의 resolver가 존재한다.

**[ Unit resolvers ]**

한방에 바로 데이터소스(DynamoDB, RDS 등)와 연결시켜 request와 response를 처리해주는 resolver 이다.

<br>

**[Pipeline resolvers]**

Unit resolvers로 해결되지 않은 복잡한 로직들을 처리하기 위한 resolver로, Pipline reolver 타입의 AppSync 에서 제공하는 Function 기능을 활용하여 공통된 로직을 한번에 처리 할 수 있다.

### **Resolver Mapping Template**

---

그럼 resolver는 어떤 언어로 작성되는 것일까 ? ? ?

resolver는 VTL(Velocity Template Engine)로 작성된다. 

VTL은 JAVA 기반의 템플릿 엔진이며 주로 웹페이지 개발시 이용된다.

AppSync resolver Mapping Template은 2 가지의 형식으로 나뉘어져 있다. 

1. Client으로 부터 들어오는 **request**를 데이터 소스로 넘겨줄 때의 형식을 정의한다.
2. Response Mapping Template으로 Client에게 데이터 **소스의 아웃풋을 전달**할 때의 형식을 정의한다.

<br>

위의 두 가지 mapping template 에서 **로직을 삽입**할 수 있게 해 주는것이 바로 **VTL의 역할**이다.

예를 들면 user에 따라 response의 내용을 필터링 하거나 값을 변경하는 등의 여러가지 작업을 수행할 수 있다.