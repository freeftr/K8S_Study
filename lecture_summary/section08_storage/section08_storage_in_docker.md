# Section 08 Storage in Docker

### Docker가 데이터를 저장하는 곳
- `/var/lib/docker/`
  - `containers/`: 컨테이너 관련 파일
  - `image/`: 이미지 관련 파일
  - `volumes/`: 볼륨 데이터

### Layered Architecture
~~~
FROM Ubuntu

RUN apt-get update && apt-get -y install python

RUN pip install flask flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
~~~

1. Ubuntu Base 이미지
2. APT 패키지 설치
3. Python 패키지 설치
4. 소스 코드 복사
5. 엔트리포인트 설정
- 각 레이어는 이전 레이어의 변경분만 저장한다.
  - 매번 전체를 다시 저장하고 구성하는 것이 아닌 **변화된 부분**만 레이어로 쌓아 올림.

~~~
FROM Ubuntu

RUN apt-get update && apt-get -y install python

RUN pip install flask flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
~~~
- 위 코드와 아래 코드를 예시를 들면 다음과 같다.
  - 바뀐 부분인 app2.py 소스 코드 부분부터 레이어를 쓴다.

### 컨테이너 실행 시 동작
- 컨테이너를 실행하면 이미지 레이어 위에 새로운 **Writeable Layer**가 생성됨.
  - 로그 파일
  - 임시 파일
  - 컨테이너 내 수정 파일
  - 한 마디로 베이스가 되는 파일들이 아닌 부수적인 것들을 관리하기 위함.
- 컨테이너 삭제 시 이 레이어도 삭제.

### Copy-on-Write
- Image 레이어는 기본적으로 read-only
- 컨테이너에서 해당 파일을 수정하면, Writeable layer에 이를 복사 후 수정한다.
  - 원본은 변하지 않음.

### Volumes
- 컨테이너가 삭제되면 Writeable layer도 사라진다. => 보존하기 위해 볼륨 사용
- Volume Mount
  - `docker volume create my_volume` -> `docker run -v my_volume:/var/lib/mysql mysql`
  - create를 안하고 run을 하면 자동으로 생성
  - 호스트 디렉토리에 종속적이지 않음.
- Bind Mount
  - 호스트의 특정 디렉토리를 그대로 컨테이너 안에 연결.
  - `docker run -d -v /home/user/data:/var/lib/mysql mysql`
  - 디렉토리나 컨테이너 안 파일 변경 사항들을 즉시 반영

---
### Storage Driver
- Docker의 레이어를 관리하고, COW, 볼륨 마운트 등을 담당.
  - AUFS
  - Overlay2
  - Btrfs
  - ZFS
- OS에 따라 기본 드라이버 달라짐.
- 볼륨 자체는 Volume Driver가 처리한다. 