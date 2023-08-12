# **Lambda와 API Gateway 환경 구성 실습**
먼저 lambda 함수를 생성해 준다. 

lamdba 함수를 생성후 아래와 같이 코드를 작성해 준다.

```markdown
import boto3 
import json

ec2 = boto3.client('ec2') # ec2 servis에 대한 client 객체 생성 (ec2 start, stop기능)

def lambda_handler(event, context):
    action = event['action'] # json 객체의 action 값(start or stop)을 받아와 저장
    instance_id = event['instance_id'] # json 객체에 저장되어 있는 Instance id 값을 받아와 저장

    if action == 'start': # action 값이 start일 경우 instance 실행
        ec2.start_instances(InstanceIds=[instance_id])
    elif action == 'stop': # action 값이 stop일 경우 instance 중지
        ec2.stop_instances(InstanceIds=[instance_id])
    
    ec2.create_tags(Resources=[instance_id], Tags=[{'Key': 'Action', 'Value': action}])
		# ec2 tage 생성 하는 메서드 + tage 값으로 action(key), action 변수에 저장된 값(Value)
		# 위를 통해 인스턴스에 action 태그를 추가하고, 태그값은 start or stop을 가지게 됨
		# (action 변수에 저장된 값임.)

    return {
        'statusCode': 200,
        'body': json.dumps('Instance ' + instance_id + ' ' + action + 'ed successfully')
    }

		# 위의 코드가 성공적으로 실행되면 상태코드 : 200 반환 
		# 그리고 body(응답 본문 값)에 json.dumps 함수(문자 열을 json 형태로 반환하는 함수)를 
		# 사용해 instance_id값과 action(start or stop)을 응답 메시지로 반환함.

```

그리고 나서 해당 lambda 함수에 API Gateway를 트리거 해준다. 

API Gateway를 생성하지 않았으므로 새로 생성하겠다.

API 유형을 선택할 수 있는데 http, rest, webSocket 이렇게 3 가지 유형이 있는데 이번 실습에서는 REST 유형으로 구축하겠다. 

REST 유형을 선택하고 다음으로 넘어가서 이름을 지정하고 생성해 준다.

그 다음 해당 API의 리소스와 메소드를 생성해 준다.  (메소드 유형 : get)

그 다음 메소드 요청 설정에 들어가 요청 본문 탭에서 application/json 콘텐츠를 추가해 주고, 

통합 요청 설정에 들어가 매핑 템플릿 탭에서 application/json 매핑 템플릿을 추가한 다음, 아래와 같은 코드를 작성 해 준다.

```markdown
{
  "action": "start",  # 이 값은 start or stop
  "instance_id": "i-09261deb31a99685a"
}
```

작성하고 나서 저장 후 테스트를 해 보면

<img width="742" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/c166068a-9214-4366-b20b-b31bced574fb">

위와 같이 status code가 200이 반환되며 해당 Instance가 실행되는걸 볼 수 있다.

<img width="813" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/630d4e6b-2d38-4cc1-a1ab-934e07a3b0ec">

<br><br>

**[ LOG 기록 ]**

<img width="218" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/c2bc704b-05a5-4686-8e64-2a8227dce646">