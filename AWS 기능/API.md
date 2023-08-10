# **API 개념 정리**

## **API 란 ? ? ?**
API는 Application Programing Interface라는 용어로써, 어떠한 응용프로그래밍에서 데이터를 주고 받기 위한 방법을 의미한다. 어떠한 특정 사이트에서 특정 데이터를 공유할 경우 어떠한 방식으로 정보를 요청해야 하는지, 어떠한 데이터를 제공 받을 수 있을지에 대한 규격들을 API라고 한다.

API는 사용하는 방법과, 그 용도에 따라 오픈 API와, 비공개 API 정보가 있다.

오픈 API는 말 그대로 어떠한 사용자라도 정보에 접근할 수 있으며 그 데이터를 쉽게 공유하기 위해 만들어진 규격이며, 비공개 API는 권한이 있는 일부 사용자들에게만 정보를 제공하기 위해 만들어진 규격이다.

<br>

**[ API 장점 ]**

- **데이터 접속의 표준화와 편의성**
  
    API는 모든 접속을 표준화하기 때문에 디바이스/운영체제 등과 상관없이 조건만 맞다면 누구나 동일한 액세스를 약속한다. 또한 기업에서 애플리케이션을 개발할 때 기능적 API를 사용하면, 필요한 기본 기능들(인증, 통신, 위치확인.. 등)을 매번 업데이트할 필요 없이 손쉽게 이용할 수 있다.
    
<br>

- **자동화와 확장성** 
  
    API를 통한 CRUD 처리에 따라 관련 데이터와 콘텐츠가 자동으로 생성되며 사용자의 환경에 맞춰서 정보가 전달 되기 때문에 개발 워크플로우가 간소화되며, 애플리케이션 확장이 다소 용이하다.

<br>

- **적용력**
    
    API는 변화 예측에도 큰 도움이 되기 때문에 API를 통해 데이터를 수집하고 전달하는 데 있어 유연한 서비스 환경을 구축할 수 있다.
    
<br>

**[ API 단점 ]**

- 보안성과 HTTP 방식의 제한
    
    API의 단일 진입점인 API Gateway는 해커의 타겟 대상이 될 수 있다.
    
    API의 장점 중 하나인 평범한 HTTP 메서드를 사용하여 액세스 할 수 있다는 점이, 보안성 측면에서는 큰 단점이 된다. 이 때문에 다른 일반적 인터넷 기반 리소스와 마찬가로 메시지 가로채기 공격(man-in-the-middle) CSRF(Cross-Site Request Forgery attack) 공격..등에 취약하다. 또한 이러한 HTTP method는 메서드 형태가 다소 제한적이라는 문제점이 있다.

API Gateway란 규모에 상관없이 API 생성 및 유지 관리, 모니터링과 보호를 할 수 있게 해주는 서비스.

Client에서 server로 통신할 때 사용하는 많은 api들의 대문과 같은 역할을 한다고 보면 된다.

(즉, API 가 지나가는 통로라고 볼 수 있다.)

<br>

## **API Gateway를 사용하는 이유 ?**

API Gateway를 이용하면 통합적으로 REST API와 엔드포인트를 관리할 수 있다.

API Gateway를 등록해주면, 모든 Client는 각 서비스의 엔드포인트 대신 API Gateway로 요청을 전달하여 관리가 용이해 진다. 그 이유는 바로 사용자가 설정한 라우팅 설정에 따라 각 엔드포인트로 Client를 대신하여 요청하고 응답을 받은후 다시 Client에게 전달하는 프록시(proxy) 역할을 하기 때문이다.

API Gateway 서비스는 단순히 api 경유지 역할 뿐만 아니라, 엔드포인트 서버에서 공통으로 필요한 인증/인가, 사용량 제어, 요청/응답 변조 등의 다양한 기능을 플러그인 형태로 제공하고 있다.

이러한 플러그인 API Gatway를 사용하면, 각 엔드포인트 서버마다 위의 기능들을 구현하지 않아도 되기 때문에 개발자 입장에서는 개발 비용을 줄일 수 있다는 효과가 있다.

(특히 **API Gateway**를 통해 **Lambda와 연동**하여 **Serverless 서비스를 구축**하는데 많이 사용된다.)

API Gateway에서 제공하는 API는 대표적으로 3종류가 있다.

- **HTTP API**
- **REST API**
- **WebSocket API**

<br>

### **[ HTTP API ]**

HTTP API는 엔드포인트를 API Gateway로 활용하여 **HTTP 요청을 통해서** **서버에 접근**할 수 있도록 만들어 준다. 그리고 HTTP API는 **데이터만 주고 받고** **UI 화면이 필요하면 Client가 별도로 처리**한다. 대부분 앱웹/ 서버 to 서버에서 사용된다.  

<img width="815" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/d4e128aa-86b3-40cb-afd8-ab24e8c2a723">


(request를 날리면 request에 대한 데이터를 반환한다. 즉, 데이터만 주고 받는 것을 HTTP API 라고 함.)

<br>

### **[ REST API ]**

REST API는 HTTP API에 여러가지 제약 조건이 추가된 형태이다.

그리고 REST API는 아래와 같은 4가지 제약 조건을 만족해야만 REST FUL하다라고 말할 수 있다.

- **Client-Server 분리**
- **무상태성**
- **캐시 가능성**
- **일관된 인터페이스**

또한 REST API는대표적으로 **CRUD 메서드의 동작**을 일컫는다. 

- **Create(post)**
- **Read(get)**
- **Update(put)**
- **Delete(delete)**

<img width="813" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/6ae7c558-bd34-4003-9a4e-a9aebf7abc9a">


<br>

### **[ WebSocket API ]**

WebSocket API는 요청하고 응답하는 REST API와 달리, Client 앱과 Backend 간의 양방향 통신을 지원한다.

주로 채팅 앱 및 스트링 대시보드와 같은 실시간 양방향 통신 애플리케이션을 구축하여 Backend 서비스와 Client간의 메시지 전송을 처리하기 위해 지속적인 연결을 유지한다.