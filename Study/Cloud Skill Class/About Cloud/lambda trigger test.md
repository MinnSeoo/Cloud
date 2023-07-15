# **Aws Lambda와 EventBridge를 사용하여 예약기반 람다 함수 만들기!** 

무현t가 내 주신 과제 중에 예약기반 람다 함수를 만들어 Ec2를 자동으로 실행 및 중지 시키는 과제가 있었다. 그래서 그 과제를 풀면서 한번 정리 해 보았다.

## **lambda IAM 생성하기**
첨에 아무것도 모르고 냅다 람다 함수 부터 작성을 하고 테스트를 해 보니 자꾸 같은 오류가 반복해서 발생하고 정상적으로 작동하지 않았다. 그 이유는 바로 lambda 함수에 대한 IAM을 지정해 주지 않아서였다.

그래서 lambda 정책을 하나 생성해 Ec2와 CloudWatch 권한을 추가하였다.

![image](https://github.com/MinnSeoo/Cloud/assets/102645965/473371d8-1607-42cf-b69b-33324296f271)




## lambda 함수

처음에 작성한 코드

```python
import boto3

region = 'ap-northeast-2'
instance = 'i-0c96b6f78026c12fa'       # 리스트 형태로 지정하지 않아서 오류 발생함.

def lambda_handler(event, context):
ec2 = boto3.client('ec2',region-name = region)
ec2.start_instance(InstanceId = instance)
print('start your instance : ' + str(instance))
```

위와 같이 코드를 작성하고 테스트를 실행시켰는데 정상적으로 실행되지 않고 오류가 발생하였다.

뭐가 문제지? 하고 생각하다가 모르겠어서 구글링을 해 보았다. 알고보니 instance의 id값을 리스트 형태로 저장해야 하는데 나는 instance의 id가 1개이니 굳이 리스트 형태로 저장해야 할까? 라고 생각하고 리스트 형태로 저장하지 않은게 문제였다. 그래서 위 코드의 3 번째 줄 에서 instance id를 리스트 형태로 변경해 주고 나서 다시 테스트를 실행하니 정상적으로 작동하였다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca7593f5-5885-4a12-a2ad-e7e9abc6485d/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e966408b-0026-480d-bfa4-4eb6503c8ce5/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4c1cfa44-dab8-4171-905a-4b708290841c/Untitled.png)

위 의 코드는 start-ec2 람다의 코드였고 아래는 stop-ec2 람다 코드이다.

<br>

```python
import boto3

region = 'ap-northeast-2'
instance = ['i-0c96b6f78026c12fa']

def lambda_handler(event, context):
	ec2 = boto3.client('ec2', region_name = region)
	response = ec2.stop_instances(InstanceIds = instance)
	print("Instance Stoped : " + str(response))
	
```

8,9 번 째 줄만 조금 코드가 변경되고 start-ec2와 유사하다.

코드를 테스트 해 보면 start-ec2를 테스트 돌렸을때 켜진 my-bastion-ec2가 꺼지는 것을 볼 수 있다.

