# Section 05 Commands and Arguments in Kubernetes

`docker run ubuntu-sleeper 10`

도커에서는 위와 같이 인자를 추가했다면 쿠버네티스에서는 `args`를 이용해 추가할 수 있다.
- `args`는 CMD를 덮어씌우는 역할을 한다.

ENTRYPOINT는 `command`로 덮어 씌울 수 있다.
