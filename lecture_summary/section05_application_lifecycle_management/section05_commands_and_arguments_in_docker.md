# Section 05 Commands and Arguments in Docker

Docker에서 다음의 명령어를 입력해보자.
- `docker run ubuntu`

그럼 컨테이너는 즉시 실행되고 바로 종료된다. Ubuntu는 bash 쉘을 실행하는 것인데, 이 bash는 입력 없이 실행되면 바로
종료되기 때문이다. 컨테이너는 해당 프로세스가 종료되면 같이 종료된다.

컨테이너가 실행하는 기본 프로세스는 Dockerfile안의 CMD 또는 ENTRYPOINT로 지정할 수 있다.
- `docker run` 뒤에 실행할 명령어 추가

## CMD & ENTRYPOINT
***
위에서 뒤에 추가하는 명령어를 영구 저장하여 사용할 수 있다. 

- 쉘 방식
  - `CMD sleep 5`
- JSON 배열 방식
  - `CMD ["sleep", "5"]`
  - 첫 번째 요소는 명령어, 인자는 개별 요소로 구분해서 저장

위의 도커파일 생성 후 `docker build -t ubuntu-sleeper`, `docker run ubuntu-sleeper`를 실행
- 추가적으로 뒤에 인자를 수정하여 실행하고 싶은 경우는 `sleep 10` 이런식으로 붙이면 sleep이 덮어씌워짐.

위의 방식이 지저분하다? => 도커파일에 ENTRYPOINT를 지정해서 사용할 수 있다.
- `docker run ubuntu-sleeper 10`
- `ENTRYPOINT ["sleep"]` 이와 같이 지정하면 실행 시 인자는 여기에 붙게 댐.
- 반대로 인자가 없으면 에러 발생 => 인자가 안붙은 상태로 실행되기 때문
- 그럴땐 아래와 같이 해결할 수 있다.
~~~
ENTRYPOINT ["sleep"]
CMD ["5"] => 기본값 역할을 함.
~~~