## **Flask 설치**

PUTTY로 ssh에 먼저 접속시켜준다.

그 다음 밑에 나온 명령어 순서대로 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eed822e0-ddb1-4f56-89ab-63e3263c8504/Untitled.png)

여기서 4번 가상환경 접속 후 [app.py](http://app.py) 파일을 생성 해 준다 → `vi app.py`

그리고 밑에 코드 복붙 (Hello World를 출력하는 코드) → 2번째 줄에 `_` 꼭 붙여줘야 함.

언더바 안 붙이면 아래와 같은 **오류** 발생 🔽

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/14f8a385-5963-498f-9977-7a8cf07fc8ab/Untitled.png)

[4번 가상환경 접속후 [app.py](http://app.py) 파일 생성 후 코드 넣기]

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello,World!"
```

저장 후 실행하면 flask server 돌아감 → `flask run` 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/960f8b8f-b949-448c-bbfa-7f4be45e023d/Untitled.png)

실행후  웹 사이트에 ip주소로 접속 해 보면 error 발생

이때 아래 명령어 사용 🔽

`python3 flask run -p 0.0.0.0`