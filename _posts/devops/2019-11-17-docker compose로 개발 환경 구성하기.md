---
author: changhoi

categories:
  - devops

tags:
  - docker-compose
  - docker
---

![thumbnail](https://live.staticflickr.com/778/22947137613_69a88cb94b_b.jpg)

도커에 대한 이야기는 개인적으로 정리해서 블로그에 올려 두었습니다! 이번 포스팅에서는 이론적인 얘기 보다 저희 개발팀에서 OS에서 최대한 독립적인 개발 환경을 구축하기 위해서 Docker와 docker-compose를 활용한 과정을 정리해두려고 합니다.

## Docker & docker-compose 설치

Docker와 docker-compose는 Docker를 설치하면서 함께 설치가 됩니다. 리눅스는 Docker를 설치하기가 상당히 간단했습니다. 하지만 윈도우는 최신 버전의 Docker를 설치하려면 Edition이 Education이거나 Pro여야 해서 한양대학교에서 학생 복지로 제공해주는 Window 10 Education으로 업데이트 한 다음 Docker 최신 버전을 설치할 수 있었습니다.

### Linux 설치

1. apt 패키지 관리자 패키지 목록 업데이트

```bash
sudo apt update -y
```

2. 도커 CE가 의존 중인 여러 패키지 설치

```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

3. 도커 패키지 저장소를 apt에 등록

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \ "deb [arch=amd64] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) \ stable"
```

4. 다시 패키지 목록 업데이트

```bash
sudo apt update -y
```

5. 설치할 버전 정해서 도커 CE 설치

```bash
sudo apt install -y docker-ce
```

#### 추가 작업

기본적으로 리눅스에서 설치하면 유저가 `docker user group`에 속해있지 않기 때문에 명령어를 다룰 때 `sudo`를 반드시 붙여야 합니다. 그렇게 하기 싫다면 유저를 `docker user group`에 넣어줘야 합니다. 아래와 같은 방법으로 유저를 `docker user group`에 추가할 수 있습니다.

```
sudo usermod -aG docker ${USER}
```

로그아웃 후 다시 로그인 해야 하고, 실행 시 `config.json` 권한 에러가 나는 경우에는 아래 명령어가 추가적으로 더 필요합니다.

```
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "/home/$USER/.docker" -R
```

### Window & Mac 설치

윈도우나 맥은 비슷하게 [도커 허브](https://hub.docker.com/?overlay=onboarding)에서 인스톨 프로그램을 다운 받아서 설치할 수 있는 것 같습니다. 다만 위에서 언급한 것처럼 최신 버전의 Docker를 사용하기 위해서는 윈도우 에디션이 Pro, Education이어야 합니다.

## Dockerfile 정의하기

docker-compose를 사용할 예정이기 때문에 특별히 많은 정의가 필요하지 않았습니다. 사실 필요한 것만 적으면 더 간단해질 것 같은데, docker-compose에서 실행할 때와 Dockerfile에서 정의했을 때와 차이점 몇 가지를 살펴보느라 조금 더 늘어났던 것 같습니다.

```yaml
FROM node:12

WORKDIR /app

COPY . /app

EXPOSE 3000

CMD ["npm", "start"]
```

Dockerfile에 대한 설명은 여기 [링크](<https://changhoi.github.io/posts/docker/Docker%EB%A1%9C-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0-(1)>)에서 더 자세하게 다루고 있습니다.

## docker-compose.yml 파일 정의하기

간단한 개발은 Dockerfile과 조금 디테일한 docker 명령어로 해결이 가능합니다. 예를 들어서 지금 카카오톡 플러스친구 챗봇 API 개발하는 건 현재까지 기획에 따르면 외부에 있는 Open API를 주로 활용하고, 별도의 DB도 필요가 없어서 그냥 단순한 node 앱만 Docker에 올리면 되는 상황입니다. 그러한 경우엔 Dockerfile에 저런식으로 올리고, 이미지를 빌드한 다음, `docker container run -it --rm -v ${PWD}:/app weehan/chat-bot:latest` 이런 식으로 컨테이너를 동작시키면 개발 환경에서 정상 작동 하겠지만, 단순하게 Database 하나만 붙더라도 Docker 하나씩 컨트롤 하는 방식으로는 복잡도가 상당히 올라갑니다.

예를 들어서 MongoDB 하나만 더 붙여서 개발 환경이 잘 동작하게 하려면, MongoDB 컨테이너를 실행하는 터미널이 하나 필요하고, 노드 앱을 실행하는 터미널이 하나 더 필요하게 됩니다. 게다가 순서를 맞추지 않으면 노드 앱은 에러를 만들겠죠? 또한 각 컨테이너를 링크 해줘야 하고 env 설정도 각자 해줘야 합니다. 가정만으로도 벌써 짜증이 나네요.

그렇다고 해도 Docker의 Cross OS 개발 환경은 포기하고 싶지가 않죠. 아무튼 위 같은 상황에서 쉽게 사용할 수 있는 것이 바로 docker-compose입니다. docker-compose에 대한 자세한 설명은 별도로 정리해서 글을 작성해볼 예정입니다. 이번 글은 위에서 말씀드린 대로 결국 어떻게 했는지에 집중하도록 하겠습니다.

먼저 결과적으로 나온 yml 파일은 아래와 같습니다.

```yml
version: '3'

services:
  nodeapp:
    build: .
    command: npm run dev
    ports:
      - '3000:3000'
    volumes:
      - ./:/app

    depends_on:
      - hands-on-mongodb
    environment:
      - DATABASE_URL=mongodb://hands-on-mongodb:27017/myblog

    container_name: 'node-crud-app'

  hands-on-mongodb:
    image: mongo
    restart: always
    volumes:
      - /data/db:/data/db

    container_name: 'mongodb'
```

서비스 단위로 나눠서 살펴보겠습니다. 우선 `nodeapp`은 Node service를 말하고, `hands-on-mongodb`는 MongoDB인데, 명령어에서 어떻게 적용되고 있는지 명확하게 하려고 이름을 앞에 더 가져다 붙였습니다.

```yml
services:
  nodeapp:
    build: .
    command: npm run dev
    ports:
      - '3000:3000'
    volumes:
      - ./:/app

    depends_on:
      - hands-on-mongodb
    environment:
      - DATABASE_URL=mongodb://hands-on-mongodb:27017/myblog

    container_name: 'node-crud-app'
```

- `command`: Dockerfile에서는 `npm start`가 CMD에 있었지만, command로 개발 환경의 명령어로 덮어 씌웠습니다.
- `volumes`: Dockerfile은 `VOLUME`을 설정할 때 상대 경로로 표시할 수가 없는데, docker-compose는 간단하게 허용해줍니다.
- `depends_on`: mongodb보다 앞서지 말라는 뜻입니다. 순서도 정의할 수 있습니다. `services`에 정의한 이름을 가져다 쓰면 됩니다.
- `environment`:URL을 설정할 때 `mongodb://hands-on-mongodb:27017/myblog` 이런 식으로 별다른 링크 없이도 `services`에 정의된 이름으로 네트워크 접근이 가능합니다. 하나의 네트워크를 구성해주기 때문인데 이와 관련해서는 공식 문서에 [Network](https://docs.docker.com/compose/networking/) 부분에서 자세히 확인해보실 수 있습니다.

DB 이미지 부분도 확인해보도록 하죠.

```
services:
  ...

  hands-on-mongodb:
    image: mongo
    restart: always
    volumes:
      - /data/db:/data/db

    container_name: 'mongodb'
```

- `restart`: 컨테이너를 실행할 때, 이미 실행 중인 경우 무조건 재시작 하도록 합니다.
- `volumes`: 로컬 환경의 data를 같이 묶어줍니다.

## Data volume 사용해서 Persistant data를 관리하기

한 가지를 더 고치고 싶은 상황입니다. 저는 완전히 독립적인 개발 환경이 되면 좋겠는데, DB가 호스트와 공유 중인 `/data/db`는 OS에 의존적입니다. 예를 들어서 지금 `/data/db` 디렉토리가 없다면 에러가 나겠네요. 사소할 수는 있지만, 최대한 Docker가 개발 환경을 관리하도록 하기 위해서 Docker volume을 사용해 데이터 저장하는 가상 디스크를 사용해보려고 합니다.

해당 설정을 위해서 docker-compose.yml에 다음 몇 줄이 추가됐습니다.

```yml
version: '3'

services:
  nodeapp:
    build: .
    command: npm run dev
    ports:
      - '3000:3000'
    volumes:
      - ./:/app

    depends_on:
      - hands-on-mongodb
    environment:
      - DATABASE_URL=mongodb://hands-on-mongodb:27017/myblog

    container_name: 'node-crud-app'

  hands-on-mongodb:
    image: mongo
    restart: always
    volumes:
      # 변경된 부분
      - data-volume:/data/db

    container_name: 'mongodb'

# 추가된 부분
volumes:
  data-volume:
```

이제 data-volume이라는 docker volume을 이용해서 Persistant data를 관리할 수 있습니다.
