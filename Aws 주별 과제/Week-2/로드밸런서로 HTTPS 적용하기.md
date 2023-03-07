## 로드밸런서로 HTTPS 적용하기

1. CDK 통해서 실습환경 구축하기 🔽
    1. 도메인 이름, 호스트존 ID, 키페어는 본인걸로 지정하기
    
    > cdk deploy elb --parameters domainName=DOMAIN_NAME --parameters hostedZoneId=HOSTED_ZONE_ID --parameters keyPair=KEYPAIR_NAME
    > 
    
2. Outputs에서 인스턴스 ID, ALB 이름 및 ALB와 연동된 도메인 주소를 확인

1. Outputs에서 나온 URL로 접속해서 정상 작동하는지 확인

1. AWS Certificate Manager에서 DNS Validation을 통해서 SSL 인증서를 발급받기 
    1. Aws ceritificate Manger 검색 후 인증서 요청 
    2. 인증서 유형 : public 
    3. 도메인 이름 : Rout53에 등록된 [ts.awscloud.work](http://ts.awscloud.work) 호스팅 영역 선택 후 elb를 생성하면서 자동으로 만들어진 레코드 이름과 동일하게 넣어준다.
    4. 나머지는 default 값으로 두고 요청한다
    5. DNL로 인증을 받는다.
    6. ?
    
     
    
2. 어플리케이션 로드밸런서에 HTTPS 리스너를 한다
    
    (인증서 발급받으면 Default로 되어있었음..? Case By Case)
    
    > 아래의 웹 사이트를 참고하자!
    > 
    
    [Application Load Balancer를 사용하여 HTTP를 HTTPS로 리디렉션](https://aws.amazon.com/ko/premiumsupport/knowledge-center/elb-redirect-http-to-https-using-alb/)