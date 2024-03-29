# **Aws Lambda와 EventBridge를 사용하여 예약기반 람다 함수 만들기!** 

무현t가 내 주신 과제 중에 예약기반 람다 함수를 만들어 Ec2를 자동으로 실행 및 중지 시키는 과제가 있었다. 그래서 그 과제를 풀면서 한번 정리 해 보았다.

## **lambda IAM 생성하기**
첨에 아무것도 모르고 냅다 람다 함수 부터 작성을 하고 테스트를 해 보니 자꾸 같은 오류가 반복해서 발생하고 정상적으로 작동하지 않았다. 그 이유는 바로 lambda 함수에 대한 IAM을 지정해 주지 않아서였다.

그래서 lambda 정책을 하나 생성해 Ec2와 CloudWatch 권한을 추가하였다.

![image](https://github.com/MinnSeoo/Cloud/assets/102645965/473371d8-1607-42cf-b69b-33324296f271)


<br>

## **lambda 함수**

처음에 작성한 코드

```python
import boto3

region = 'ap-northeast-2'
instance = 'i-0c96b6f78026c12fa'    # 리스트 형태로 지정하지 않아서 오류 발생함.
def lambda_handler(event, context):
	ec2 = boto3.client('ec2', region_name = region)
	response = ec2.stop_instances(InstanceIds = instance)
	print("Instance Stoped : " + str(response))

```

위와 같이 코드를 작성하고 테스트를 실행시켰는데 정상적으로 실행되지 않고 오류가 발생하였다.

뭐가 문제지? 하고 생각하다가 모르겠어서 구글링을 해 보았다. 알고보니 instance의 id값을 리스트 형태로 저장해야 하는데 나는 instance의 id가 1개이니 굳이 리스트 형태로 저장해야 할까? 라고 생각하고 리스트 형태로 저장하지 않은게 문제였다. 그래서 위 코드의 3 번째 줄 에서 instance id를 리스트 형태로 변경해 주고 나서 다시 테스트를 실행하니 정상적으로 작동하였다.

<img width="1590" alt="스크린샷 2023-07-14 오후 1 32 34" src="https://github.com/MinnSeoo/Cloud/assets/102645965/aded5def-a177-4667-9b75-371a365f4152">

<br>

![image](https://github.com/MinnSeoo/Cloud/assets/102645965/7a54ef26-dd45-4b37-82c4-60ffc9c3946d)


![image](https://github.com/MinnSeoo/Cloud/assets/102645965/1e703da1-fce0-4d8b-8b14-34829416e669)

<br>


위 의 코드는 start-ec2 람다의 코드이고 아래는 stop-ec2 람다 코드이다.

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

<br>

이제 start-ec2, stop-ec2의 람다 함수를 eventbridge로 트리거 하여 특정시간에 자동으로 실행될 수 있도록 하겠다. 

start-ec2, stop-ec2의 람다 함수에 트리거 추가 버튼을 눌러 eventbridge의 규칙을 생성해 준다.

과제에서 cron 표현식을 사용하라고 했기 때문에 eventbridge scheduler에서 계속하기 버튼을 클릭한 후 
이름을 적어주고 일정 패턴을 지정하는 부분에 반복 일정의 cron 기반 일정을 선택하여 cron 표기법에 맞게끔 내가 원하는 시간을 지정해 주고 생성 버튼을 클릭한다.

그럼 이제 내가 지정한 시간에 start, stop 함수가 실행되는걸 볼 수 있다.

아래 img 들은 event들이 발생한 기록들을 기록해 두는 log img 이다.

### **[ec2-start]**
<img width="743" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/9b33960a-4589-4b56-8259-2cabe97ac73f">

<br>

### **[ec2-stop]**
<img width="814" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/16211056-84df-41ee-a09b-37dcbff3eb91">

<br>

**[아래 img는 4일 동안 eventbridge가 실행된 기록을 보여주는 모니터링 화면이다]**

<img width="1372" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/ff5db3cb-080d-4120-b2c0-c322443f77fd">
