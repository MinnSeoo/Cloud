## **Terraform 이란 ? ? ?**

- 하시코프에서 오픈소스로 개발중인 클라우드 Infrastructure 관리 도구이다.
- 테라폼은 코드로서의 Infrastructure를 지향하고 있는 도구로서, GUI나 웹 콘솔을 사용해 서비스 실행에
필요한 리소스를 관리하는 대신 필요한 리소스들을 **선언적인 코드로 작성**해 **관리**할 수 있도록 지원한다.

## **프로비저닝**

- 어떤 프로세스나 서비스를 실행하기 위한 준비 단계.
- 네트워크나 컴퓨팅 **자원을 준비**하는 작업과 준비된 컴퓨팅 자원에
사이트 패키지 혹은 **애플리케이션 의존성을 준비**하는 단계로 나뉨.

## **프로바이더**

- 테라폼과 외부 서비스를 연결해주는 기능을 하는 모듈.
- EX) AWS, GCP, Azure.. 등

## **리소스(자원)**

- 특정 프로바이더가 제공해주는 조작 가능한 대상의 최소 단위.
- Ex) AWS 프로바이더 → aws instance 리소스 타입 제공 → 리소스 타입 사용하여 EC2의 VM 리소스 선언
및 조작 가능. (EC2 인스턴스, sg, key pair…등)

## **HCL (Hashicorp Configuration Language)**

- 테라폼에서 사용하는 설정 언어.
- 테라폼에서 모든 설정과 리소스 선언은 HCL을 사용해 이뤄짐.
- 테라폼에서의 확장자 `.tf` 사용.

## **계획**

- 테라폼 프로젝트 디렉터리 아래의 모든 `.tf` 파일의 내용을 실제로 적용 가능한지 확인하는 작업.
- 테라폼에서 이를 `terraform plan` 명령어로 제공함.
- 명령어 실행시 어떤 리소스가 실행, 수정, 삭제될지 계획을 보여준다.

## **적용**

- 테라폼 프로젝트 디렉터리 아래의 모든 `.tf` 파일의 내용대로 리소스를 생성, 수정, 삭제하는 작업.
- 테라폼에선 이를 `terraform apply` 명령어로 제공함.
- 위 명령어를 실행하기 전 변경 예정 사항은 `plan`  명령어를 통해 확인 가능.
- 적용하기 전에도 plan의 결과를 보여줌.

## **AWS 프로바이더 정의**

먼저 프로젝트 디렉토리와 테라폼 파일 몇 개 생성하기.

```markdown
# ts_test 폴더 생성
$ mkdir ts_test 

# ts_test 폴더로 이동
$ cd ts_test

# touch 명령어로 빈 파일 생성
$ touch provider.tf ts_test.tf
```

디렉토리 이름과 파일 명에 대한 특별한 원칙은 없다.

테라폼은 기본적으로 특정 디렉토리에 있는 모든 `.tf` 확장자를 가진 파일을 전부 읽어들인 후, 리소스 생성, 수정, 삭제 작업을 진행한다. 

(참고로 테라폼의 파일 확장자 → `.tf` 이다. )

먼저 `provider.tf`  파일 작성해준다. `.tf` 확장자의 파일은 **HCL 언어**로 작성 된다. 

HCL로 다음과 같이 AWS 프로바이더를 정의한다.

```markdown
provider "aws" {
  access_key = "<AWS_ACCESS_KEY>"
  secret_key = "<AWS_SECRET_KEY>"
  region = "ap-northeast-2"
}
```

이때 ACCESS_KEY와 SECRET_KEY는 내가 따로 생성한 terraform 사용자의 인증 정보로 대체하겠다.

프로바이더 선언은 아래와 같은 형식을 따른다.

> **provider “<PROVIDER_NAME>”**
> 

이번 실습에서는 aws 프로바이더를 사용하기 때문에 **<PROVIDER_NAME>** 자리에 aws를 넣었다.

프로바이더 선언 뒤로는 중괄호 블록이 따라올 수 있으며, 중괄호 사이에는 프로바이더에서 사용가능한 하나 이상의 속성을 지정할 수 있다.

```markdown
provider "<PROVIDER_NAME>" {
  <ATTR_NAME> = "<ATTR_VALUE>"
}
```

이번 실습에서는 3가지 속성만 지정했지만, aws 프로바이더는 더 다양한 속성들을 지원한다.

## **환경변수로 AWS 프로바이더 설정하는 방법**

위에서 AWS 프로바이더를 설정할 때 파일 안에 액세스 키 및 시크릿 키를 기록하였다.

테라폼은 코드로 작성되기 때문에 다른 프로그래밍 언어 소스 코드와 마찬가지로 Git과 같은 버전 관리 도구에서 관리하는게 일반적이기 때문에 민감한 정보를 저장소에 기록해서는 안된다.

이러한 문제를 피하기 위해서는 테라폼을 실행하는 환경에 직접 환경변수로 정의하는 방법을 사용할 수 있다.

이 때 사용하는 환경변수는 AWS CLI에서 사용하는 환경 변수와 같다.

**[ 아래 코드 작성시 띄어쓰기 주의!! ]**

```markdown
$ export AWS_ACCESS_KEY_ID="<MY_ACCESS_KEY_ID>"
$ export AWS_SECREAT_KEY="<MY_SECRET_ACCESS_KEY>" 
$ export AWS_DEFAILT_REGION="ap-northeast-2"  # 서울 리전 
```

## **테라폼 프로젝트 초기화**

---

`terraform init` 명령어를 통해 테라폼 프로젝트를 초기화 한다. 

이때 프로바이더 설정을 보고 필요한 플러그인들을 설치한다.

(테라폼 프로젝트가 있는 위치에서 init 명령어를 실행해야 함.)

그리고 나서 `terraform version` 명령어를 실행하면 테라폼 프로젝트에서 사용중인 프로바이더들의 버전들도 함께 보여준다.

```markdown
minnseoo@jeongminseoui-MacBookPro tf_test % terraform version
Terraform v1.5.5
on darwin_arm64
+ provider registry.terraform.io/hashicorp/aws v5.12.0
```

### **[ 테라폼의 버전과 프로바이더의 버전 ]**

위의 실행 결과를 보면 테라폼 버전과 프로바이더 버전이 따로 출력된다는 것을 알 수 있다.

따로 출력된다는 것은 **버전이 따로 관리**된다는 의미이다.

이는 테라폼과 프로바이더 버전에 따라서 미묘하게 실행 결과가 다른 경우가 많이 발생하기 때문에 가능하면

테라폼과 프로바이더의 버전을 프로젝트에 고정해서 사용하고 명시적으로 업데이트하는 것을 권장하고 있음.

## **[ 1 ] -   EC2 용 SSH 키 페어 정의**

---

EC2 인스턴스를 생성하기 위해서는 키 페어가 필요하다. AWS 상에서 EC2를 생성해도 keypair를 지정하지 않으면 EC2 서버에 접속할 수 없게 된다. 따라서 인스턴스를 생성하기 전에 미리 키 페어를 생성해야 한다.

### **[ 1. HCL 언어로 필요한 리소스를 정의 ]**

```markdown
resource "aws_key_pair" "키페어_명" {
  key_name = "키페어_명"
  public_key = "<public_key>"
}
```

위 코드에서 중요한 점은 resource 키워드 다음에 “aws_key_pair” “키페어_명”와 같이 두개의 문자열이 온다는 점이다.

첫 번째 문자열("aws_key_pair")은 리소스 타입의 이름으로, 이 자리에 올 수 있는 값들은 프로바이더에서 제공하는 리소스 타입의 이름만 올 수 있다. → ex) keypair, instance, access_key..등이 있다.

두 번째 문자열(”키페어_명”)은/는 이 리소스에 임의로 붙이는 이름으로 테라폼 코드의 다른 곳에서 이 리소스를 참조하기 위해서 사용한다. 리소스 타입과 이름을 `.` 으로 이어 `aws_key_pair.MyKey` 와 같은 형식으로 참조한다.

<aside>
😋 단, 여기서 지정하는 이름은 AWS에서 사용하는 이름이 아니기 때문에
AWS에서 정의되는 리소스 이름과 혼동해서는 안된다.

</aside>

위의 코드에서 public_key 부분에는 반드시 공개키의 값만 와야한다. 만약 로컬에 미리 생성해 둔 공개키가 없을 경우 아래와 같은 명령어를 사용하여 생성할 수 있다. ( 이렇게 하면 공개키와 개인키 파일이 생성된다. )

```bash
$ ssh-keygen -t rsa -b 2048 -C "your_email@example.com"
```

그리고 public_key 부분에 공개키 값이 오게 되는데 이 공개키 값이 길기 때문에 `file()` 함수를 사용하여 공개키 경로를 지정해 주면 공개키 값을 문자열로 읽어온다. (아래와 같이 작성한다.)

```bash
resource "aws_key_pair" "키 페어 이름" {
  key_name = "키 페어 이름"
  public_key = file("로컬에 존재하는 키 페어 경로")
}
```

## 2.  **선언한 리소스들이 생성가능한지 계획(plan)을 확인**


첫 번째 step에서 필요한 리소스들을 정의하였다. 이제 이 리소스들을 바탕으로 AWS에 생성할 수 있는지 확인해야한다.

프로젝트 디렉토리에서 `terraform plan` 을 실행한다. `plan` 명령어를 사용하면 현재 정의되어 있는 리소스들을 실제 프로바이더(여기선 AWS)에 적용했을 때, 어떤 작업을 수행하는지 계획을 보여준다. 

```bash
❯ terraform plan
...
Terraform will perform the following actions:

  # aws_key_pair.키페어_명 will be created
  + resource "aws_key_pair" "키페어_명" {
      + fingerprint = (known after apply)
      + id          = (known after apply)
      + key_name    = "키페어_명"
      + key_pair_id = (known after apply)
      + public_key  = "ssh-rsa ...."

Plan: 1 to add, 0 to change, 0 to destroy.
```

바로 위에서 작성한 리소스 코드와 비슷한 형식으로 결과가 출력된다. 각 행 앞에 존재하는  `+`  문자는 그 행의 리소스들을 생성하겠다는 의미를 가진다.

### **+ 테라폼의 동작원리 이해하기!**

---

`plan` 의 작동 방식을 이해하기 위해선 먼저 테라폼의 동작 방식을 이해해야 한다.

**테라폼 리소스들은 선언적으로 기술**된다. 이 개념은 **“무엇을” 달성**하고 **“원하는 상태”**가 **무엇**인지 설명하는 방식을 의미한다. 

```bash
resource "aws_instance" "example" {
  ami           = "ami-0123456789"
  instance_type = "t2.micro"
}
```

위 코드에서 **“무엇”**에 해당하는 부분은 `aws_instance` 로 인스턴스를 생성하고 EC2 인스턴스를 어떻게 구성해야하는지 type과 ami가 기술되어 있다 이 부분이 **“원하는 상태”**이다. 따라서 위 코드는 선언적으로 기술되어 있다고 할 수 있다.

테라폼은 `.tf` 파일에 기술되어있는 모든 리소스들을 먼저 읽어들이고 먼저 이 리소스들이 존재하는 상태를 가정한다. 편의상 이를 이상적인 상태라고 부르겠다. 

예를 하나 들어보자. 나는 `aws_key_pair.web_admin`이라는 키 페어를 내 AWS 계정에 추가하려고 한다.

그래서 나는 `aws_key_pair.web_admin` 리소스를 정의하였다. 그렇기에 이 리소스는 현재 이상적인 상태에 존재하는 리소스이다. 하지만 이 리소스는 실제로 생성된 적이 없기 때문에 프로바이더에 지정한 AWS 계정에는 존재하지 않는다. 그 말은 즉, 내 AWS 계정에는 아직 키 페어가 생성되지 않았다는 이야기다.

<img width="813" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/59ac9a03-2b09-4f86-a590-f3e33fb2b1df">


**[ 현재 위와 같은 상태이다. ]**

테라폼에서 가장 중요한 역할은 실제 상태를 이상적인 상태와 동일하게 만드는 일인데, 테라폼에서는 이 작업을 적용한다(apply)라고 한다. 테라폼 **plan**은 **apply 하기 전**에 **이상적인 상태**와 **실제 상태**를 **비교**해 **둘을 동일**하게 만들기 위해서 해야할 일을 찾아내는 작업이다. 

내가 생성하고 싶은 키 페어 리소스는 현재 이상적인 상태에만 존재하기 때문에, 실제 상태에도 `aws_key_pair.web_admin` 리소스를 생성해야 한다.

<img width="828" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/15d0e955-d282-4300-ae09-d992d04cd291">


**[ terraform plan이 보여주는 리소스 생성 계획 ]**

## 3. 선언된 리소스들을 AWS에 적용(Apply)

---

이제 plan을 통해 확인한 내용을 실제 프로바이더에 적용하겠다. 적용하기 위해서 `terraform apply` 명령어를 실행한다.

```bash
$ terraform apply
...
Terraform will perform the following actions:

  # aws_key_pair.web_admin will be created
  + resource "aws_key_pair" "web_admin" {
      + fingerprint = (known after apply)
      + id          = (known after apply)
      + key_name    = "web_admin"
      + key_pair_id = (known after apply)
      + public_key  = "ssh-rsa ..."
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:
```

실행하면 plan의 결과를 보여주며 리소스를 생성 여부를 물어본다. 이때 `yes` 를 입력해야만 리소스를 생성한다.

엥? 뭐야 apply 명령어를 실행해도 plan의 결과를 보여주는 거면 애초에 plan 명령어를 실행할 필요없이 바로 apply 명령어를 실행해도 되는거 아닌가? 라는 생각이 들었다. 

그래서 좀 더 찾아봤더니 코드 작성중에는 apply보다 plan으로 변경사항들을 확인하는 것이 더 좋고 안전한 방법이기 때문에 추천한다고 하였다.

```bash
aws_key_pair.web_admin: Creating...
aws_key_pair.web_admin: Creation complete after 0s [id=web_admin]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

정상적으로 aws_key_pair.web_admin 리소스가 생성된 것을 확인할 수 있다. 

( AWS 내 계정에서도 확인 가능하다! )

이렇게 apply 명령어를 실행함으로서 이상적인 상태에만 존재했던 `aws_key_pair.web_admin` 리소스를 실제 상태에 생성하였다. 이로서 이상적인 상태와 실제 상태를 동일하게 만들었다.

<img width="809" alt="image" src="https://github.com/MinnSeoo/Cloud/assets/102645965/008f7e07-c236-4a67-8c74-c46bd5146bfd">


## **[ 2 ] -   SSH 접속 허용을 위한 SG**

---

두 번째로 정의할 리소스는 `aws_security_group` 으로 keypair와 마찬가지로 인스턴스를 정의하기 위해서 필요한 리소스이다. 

만약 인스턴스를 생성했는데 외부에서 접근할 수 없다면 사용할 수 없다. 따라서 SSH port를 외부에서 열어주는  보안그룹을 만들어야 한다.

```bash
# web_infra.tf

resource "aws_security_group" "ssh" {
	name = "Allow SSH"
	description = "Allow SSH Port"
	ingress {
		from_port = 22
		to_port = 22
		protocol = "tcp"
		cidr_blocks = ["0.0.0.0/0"]
	}
}

```

- **resource** : 리소스 타입은 `“aws_security_group"`
- **테라폼 상에서 이름** : `“ssh”`
- **description** : 설명 할 내용 적어주기
- **ingress** : 인바운드(들어오는) 트래픽에 대한 규칙을 정의하는 부분.
- **from_port** : 인바운드 트래픽을 허용되는 시작 포트 → 이번 실습에서는 22번 부터
- **to_port** : 인바운드 트래픽이 허용되는 끝 포트 → 이번 실습에서는 22번 포트까지
- **protocol** : 인바운드 트래픽에 적용되는 프로토콜 지정 → 이번 실습에서는 “tcp” 프로토콜 사용!
- **cidr_blocks** : 인바운드 트래픽을 허용할 출처 IP 주소 범위 지정 → 이번 실스베서는 모든 ip주소 허용!

여기서 특이한 점은 ingress 속성에 직접 값을 지정하는 대신 중괄호 블록이 따라온다는 점이다.

그리고 ingress 속성이 하나 이상 올 수 있으며, 필요에 따라서 아웃바운드 트래픽을 제어하는 `egress` 속성도 사용할 수 있다. 

ingress 블록 안에는 **from_port** , **to_port , protocol , cidr_blocks** 속성을 지정한다.

이제 plan을 실행하자!

```bash
$ terraform plan
...
  # aws_security_group.ssh will be created
  + resource "aws_security_group" "ssh" {
      + arn                    = (known after apply)
      + description            = "Allow SSH port from all"
      + egress                 = (known after apply)
      + id                     = (known after apply)
      + ingress                = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = ""
              + from_port        = 22
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "tcp"
              + security_groups  = []
              + self             = false
              + to_port          = 22
            },
        ]
      + name                   = "allow_ssh_from_all"
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + vpc_id                 = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

실행 결과를 보면 하나의 보안그룹이 추가될 예정이라는 것을 확인할 수 있다. 그럼 이제 계획을 적용해 이상적인 상태와 실제 상태를 같게끔 만들어 준다.

```bash
$ terraform apply
aws_security_group.ssh: Creating...
aws_security_group.ssh: Creation complete

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

## [ 3 ] - EC2 인스턴스 정의

---

앞서 정의한 키 페어와 보안 그룹으로 이제 인스턴스를 정의할 차례이지만, 그 전에 VPC의 기본 보안 그룹을 불러오겠다. 테라폼은 AWS 상에 이미 정의된 리소스를 불러오는 기능을 제공한다. 

`aws_security_group` 데이터 소스를 사용해, 미리 정의된 보안 그룹을 불러 올 수 있다.

```bash
data "aws_security_goup" "default" {
	name = "default"
}
```

앞서 작성한 리소스 코드와 비슷하지만, 자세히 보면 resource가 아닌 data로 시작하는 것을 확인할 수 있다.

이는 데이터 소스 이름이 “default”인 보안 그룹을 찾아 해당 리소스의 속성을 참조할 수 있게끔 해 준다.

자! 이제 인스턴스를 정의해보겠다. 참고로 EC2 인스턴스를 정의하는 리소스는 `aws_resource` 이다.

```bash
resource "aws_instance" "web" {
	ami = "ami-number"
	instance_type = "t2.micro"
	key_name = aws_key_pair.web.admin.key_name
	vpc_security_group_ids = [
		aws_security_group.ssh.id,
		data.aws_security_group.default.id
	]
}
```

- **테라폼에서 사용하는 인스턴스 리소스 명** : “web”
- **ami** : 인스턴스 ami
- **instance_type** : 인스턴스 타입 지정
- **key_name** : EC2 키 페어 이름을 지정함.
→ 여기선 변수를 사용해 앞서 정의한 `aws_key_pair.web_admin` 리소스에 key_name이라는 속성을 참조하는 방식을 사용했다. (이 부분은 코드 설명후 나중에 다시 설명하겠다.)
- **vpc_security_group_ids** :  EC2 인스턴스의 보안 그룹을 나타내는 속성
    
    → **`aws_security_group.ssh.id` :** EC2 인스턴스에 적용할 첫 번째 보안 그룹으로, `**aws_security_group**`  리소스의 이름이 `**ssh**` 인 보안 그룹의 ID를 참조하고 있다.
    
    → `**data.aws_security_group.default.id`** 이 부분은 EC2 인스턴스에 적용할 두 번째 보안 그룹으로, 
    default VPC 리소스를 AWS에서 가져온다.
    
    **[ key_name에서 설명하려고 했던 내용을 이어서 설명하겠다. ]**
    
    앞서 정의한 `web_admin` 을 문자열로 지정할 수도 있지만, 굳이 위와 같이 변수를 사용해 리소스의 속성 값을 참조하는 방식을 사용하였다.
    
    여기에는 두 가지 이유가 존재한다. **첫 번째 이유**는 어떤 리소스를 정의할 때 다른 리소스들의 속성을 참조할 수 있음을 보여주기 위해서이고, **두 번째 이유**는 테라폼은 (코드순서가 아닌) 그래프 모델로 이러한 의존 관계를 정의하고 리소스 생성 순서를 결정한다. 
    
    **따라서 위와 같이 정의한 경우 테라폼은 `aws_key_pair.web_admin` 리소스가 `aws_instance.web` 리소스보다 먼저 생성 되는 것을 보장해 준다.**
    

이제 `terraform plan` 으로 리소스 추가 계획을 확인해 보자.

```bash
$ terraform plan
  + resource "aws_instance" "web" {
      + ami                          = "ami-0a93a08544874b3b7"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
      + availability_zone            = (known after apply)

Plan: 1 to add, 0 to change, 0 to destroy.
```

그리고 `terraform apply` 로 바로 적용시켜보겠다.

```bash
$ terraform apply
data.aws_security_group.default: Refreshing state...
aws_key_pair.web_admin: Refreshing state... [id=web_admin]
aws_security_group.ssh: Refreshing state... [id=sg-sgId]
...
  # aws_instance.web will be created
  + resource "aws_instance" "web" {
      + ami                          = "ami-amiNumber"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
      + availability_zone            = (known after apply)
...
aws_instance.web: Creating...
aws_instance.web: Still creating... [10s elapsed]
aws_instance.web: Still creating... [20s elapsed]
aws_instance.web: Creation complete after 21s [id=i-idNumber]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

이렇게 하면 리소스가 정상적으로 생성되었다는 메세지가 출력되는 것을 볼 수 있다.

그리고 AWS 내 계정으로 들어가 EC2 서비스 창에 접속하면 인스턴스가 생성된 것을 확인할 수 있다.

마지막으로 출력된 인스턴스 ID 값으로 콘솔창에 접속해 보도록 하겠다.

```bash
$ terraform console
> aws_instance.web.public_ip
ip.number.number.number 
```

ip주소를 이용하면

```bash
$ ssh -i ~/.ssh/web_admin ec2-user@number
The authenticity of host 'number(number)' can't be established.
ECDSA key fingerprint is SHA256:48eSPznWLvWIuFkUsdudCsLJCGHIMPxHYOxq72bqdGc.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '13.124.222.81' (ECDSA) to the list of known hosts.

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
1 package(s) needed for security, out of 32 available
Run "sudo yum update" to apply all updates.
[ec2-user@ip-number ~]$
```

따란!!!!!!! 이렇게 정상적으로 ssh 창에 접속할 수 있게된다!

## [ 4 ] - RDS 인스턴스 정의

---

웹 서비스를 시작할 마지막 리소스로, AWS가 직접 관리해주는 데이터베이스 서비스 RDS의 MySQL 리소스를 생성해 보겠다.

```bash
resource "aws_db_instance" "web_db" {
	allocated_storage = 8
	engine = "mysql"
	engine_version = "8.0."
	instance_class = "db.t2.micro"
	username = "admin"
	password = "db_password"
	skip_final_snapshot = true
}
```

- **resource** : aws_db_inscance를 사용하겠다.
- **allocated_storage** : 할당할 용량 지정
- **engine** : DB 엔진을 지정한다.
- **engine_version** : 지정한 DB 엔진의 버전을 지정한다.
- **instance_class** : 인스턴스 타입을 지정한다.
- **username** : 인스턴스의 계정 이름을 지정한다.
- **password** : DB 인스턴스에 접속하기 위한 비밀번호를 지정한다.
- **skip_final_snapshot** : 인스턴스 제거시 최종적으로 스냅샷을 만들것인지 아닌지를 지정하는 옵션이다.
(default 값으로 faulse가 지정되어 있다.)

`plan` 명령어를 실행하면,

```bash
$ terraform plan
...
  # aws_db_instance.web_db will be created
  + resource "aws_db_instance" "web_db" {
      + address                               = (known after apply)
      + allocated_storage                     = 8
      + apply_immediately                     = (known after apply)
      + arn                                   = (known after apply)
      + auto_minor_version_upgrade            = true
      ...
Plan: 1 to add, 0 to change, 0 to destroy.
```

위와같이 1개의 db 인스턴스를 추가할 것이라는 계획을 보여준다. 바로 적용시켜보자.

```bash
$ terraform apply
  + resource "aws_db_instance" "web_db" {
      + address                               = (known after apply)
      + allocated_storage                     = 8
      + apply_immediately                     = (known after apply)
  ...
aws_db_instance.web_db: Still creating... [3m10s elapsed]
aws_db_instance.web_db: Still creating... [3m20s elapsed]
aws_db_instance.web_db: Still creating... [3m30s elapsed]
aws_db_instance.web_db: Creation complete after 3m36s [id=terraform-idNum]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

위와 같이 잘 적용된것을 볼 수 있다. 이제 3 ~ 5분 정도 기다리면 AWS의 RDS 메뉴에서 RDS 인스턴스가 생성된것을 볼 수 있다.

RDS가 생성되고 나면, 이번에는 테라폼 콘솔창에서 데이터베이스의 endpoint 속성을 확인한다.

```bash
$ terraform console
> aws_db_instance.web_db.endpoint
terraform-012345..
```

그럼 이제 이 주소로 RDS 인스턴스에 접속해보겠다. 먼저 앞서 생성한 EC2 인스턴스에 접속에 mysql을 설치한뒤, 위에서 출력된 엔드포인트로 접속하면 

```bash
$ mysql -h terraform-endPoint..
Enter password: 
...
mysql>
```

따단! 정상적으로 MySQL 서버에 접속이 된것을 확인할 수 있다.

이번 실습이 끝난후  **연습용**으로 만든 **리소스**를 **꼭 삭제**해 줘야한다. 

**`terraform destory` 명령어를 통해 삭제 가능하다.**