## 웹서버에 HTTPS 적용하기

1. **CDK를 통해서 실습 환경을 구축하기 🔽**
    
    아래 명령어를 통해 실습 환경 구축하기.
    
    > cdk deploy ssl --parameters domainName=DOMAIN_NAME --parameters hostedZoneId=HOSTED_ZONE_ID --parameters keyPair=KEYPAIR_NAME
    > 
    
2. **Outputs에서 인스턴스 ID, 해당 인스턴스에 설치된 웹서버와 연동된 도메인 주소 확인하기.**

1. **Outputs에서 나온 URL로 접속해서 정상 작동하는지 확인하기 🔽**
    
    URL에 접속하게 되면 배경색이 빨간색인 불안정한 웹 페이지가 보임.
    
2. 해당 웹 페이지를 참고해 HTTPS 적용하기 🔽
    
    **[Certbot 설치 준비]**
    
    1. 홈 디렉터리(`/home/ec2-user`)로 이동. 다음 명령을 사용하여 **EPEL을 다운로드**.
        
        > sudo wget -r --no-parent -A 'epel-release-*.rpm' [https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/](https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/)
        > 
    
    1. 다음 명령과 같이 **Repository 패키지를 설치**
        
        > sudo rpm -Uvh [dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-*.rpm](http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-*.rpm)
        > 
    
    1. 다음 명령과 같이 **EPEL을 활성화**
        
        > sudo yum-config-manager --enable epel*
        > 
    
    1. 다음 명령을 사용하여 EPEL이 활성화되었는지 확인
        
        > sudo yum repolist all
        > 

1. Apache 구성 파일인 `/etc/httpd/conf/app.conf`를 편집하기
    1. "`Listen 80`" 명령을 찾기
    2. 위 명령어 뒤에 ServerName, ServerAiles를 내걸로 바꾸기 
    3. 파일 저장  Apache 다시 시작하기 **( 명령어 : `sudo systemctl restart httpd`)**
    4. 이 다음도 있는데 etc 경로에 접근하려 해도 경로가 없다고 해서 못했다..