# **DockerFile 명령문**

## **FROM**

```docker
FROM <image>:<tag>

# ex) openjdk 1.8 (apline -> 태그)을 base 이미지로 사용
FROM openjdk:8-apline

```

Docker 이미지에는 base 이미지가 존재해야 한다.

base 이미지에 또 다른 이미지를 올리고 여러 개의 Image Layer를 쌓아가며 만들 수 있다.

(보통은 맨 위에 선언해준다.)

## **WORKDIR**

```docker
# ex) 작업 디렉토리를 /app/bin으로 설정
WORKDIR /app/bin
```

`WORKDIR` 명령어는 쉘(shell)에서 cd 명령어 처럼 컨테이너 상에서 작업 디렉토리 설정을 위해서 사용되는 명령어 이다.  `WORKDIR` 로 작업 디렉토리를 옮기면 해당 디렉토리 기준으로 앞으로 사용하는 명령어가 실행된다.

## **RUN**

```docker
# ex)
RUN npm install
```

`RUN` 명령어는 쉘(shell)에서 커맨드를 실행하는 명령어 이다. 이미지 Build 과정에서 필요한 커맨드를 실행 하기 위해 사용된다. (보통 이미지 안의 특정 sw을 설치하기 위해 사용된다.)

## **ENTRYPOINT**

```docker
# 기본형식 (일반적으로 두 번째 형식을 사용한다.)
ENTRYPOINT <전체 명령문>
ENTRYPOINT ["<명령문>", "<파라미터1>", "<파라미터2>"]

```

`ENRYPOINT` 명령어는 img를 컨테이너로 띄울 때 항상 실행되야 하는 커맨드를 지정할 때 사용된다.

(생략할 수 있으며, 생략될 경우 커맨드에 지정된 명령어로 수행한다.)

## **CMD**

```docker
# 기본형식
CMD ["<명령문>", "<파라미터1>", <"파라미터2">, ...]
CMD[ "<파라미터3>", "<파라미터4>"]
CMD <전체 커맨드>
```

`CMD` 명령어는 해당 img를 컨테이너로 띄울 때나 default로 실행할 커맨드, `ENTRYPOINT` 명령어로 지정된 커맨드에 default로 넘길 파라미터를 지정할 때 사용된다. 

`CMD` 와 `ENTRYPOINT` 는 기본적으로 같은 동작을 수행한다. 다만  ENTRYPOINT는 명시된 내용을 바꿀 수 없지만 CMD의 경우 docker run 실행시 커맨드를 입력해 CMD에 명시된 내용을 변경할 수 있다.
(단 CMD는 Docker file 내 한 번만 적용되며, 만약 여러 개의 CMD가 작성되었을 경우 마직만 CMD만 적용된다.)

## **EXPOSE**

```docker
# 기본형식
EXPOSE <port>
EXPOSE <port>/<protocol>
```

`EXPOSE` 명령어는 host가 컨테이너로 접속하기 위한 포트를 지정해줄 수 있다.
(포트만 적으면 기본적으로 TCP가 적용된다.)

## **COPY / ADD**

```docker
# 기본형식
COPY <복사할 파일> ... <복사한 파일을 이미지에 복사할 곳>
COPY ["<파일>", ... "<복사할 곳>"]

# ex)
# hello.Docker 파일을 복사
COPY hello.Docker hello.Docker

# img 빌드한 디렉토리의 모든 파일을 컨테이너의 app/bin 디렉토리로 복사
WORKDIR app/bin
COPY . . 
```

`COPY` 명령어는 host 컴퓨터에 있는 디렉토리나 파일을 Docker FileSystem으로 복사하기 위해 사용된다. 절대경로와 상대경로 2개가 존재하는데 2개 다 사용하능하다.
(이때 상대경로를 사용할 때에는 현재 WORKDIR 가 어디있는지 확인해야 한다.)

`ADD` 명령문은 `COPY` 명령어와 다르게 일반 파일 뿐만 아니라 압축 파일이나 네트워크 상의 파일도 사용할 수 있다. (특수한 파일을 다루는게 아니라면 `COPY` 를 사용하는데 더 좋음.)

## **ENV/ ARG**

```docker
#  ENV 기본형식
ENV <KEY> <VALUE>
ENV <KEY>=<VALUE>

# ENV ex)
# server port를 기본값 8080으로 설정
ENV SERVER_PORT 8080 # (1)

# 환경변수 server port를 9090으로 설정
docker run -e SERVER_PORT=9090 ... # (2)

# ARG 기본형식
ARG <KEY>
ARG <KEY> = <default>

# ARG ex)
ARG SERVER_PORT = 8080

# Build 시 ARG 설정
docker build -- bulid-arg SERVER_PORT=1234 ...
```

`ENV` 명령어는 환경 변수를 설정할 수 있으며,  `ARG` 명령어와 다르게 컨테이너 내부에서도 접근이
가능하다.

반면 `ARG` 명령어는 img 빌드시에만 사용할 수 있다.