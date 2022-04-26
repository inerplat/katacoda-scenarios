# 동작중인 파드를 디버그

동작중인 파드에 접근하여 코드를 수정하더라도, 어플리케이션에 바로 반영되지는 않습니다.

- 컨테이너 접근

  `kubectl exec -it interpreted-lang -- /bin/bash`{{execute}}

- 소스코드 확인

  `cat index.py`{{execute}}

- "Hell World"를 "Hello World"로 수정

  `sed -i -E "s/Hell World/Hello World/g" index.py`{{execute}}

- 변경된 소스코드 확인

  `cat index.py`{{execute}}

- 어플리케이션 응답 확인

  `curl localhost`{{execute}}

재시작을 위해 ENTRYPOINT로 설정된 프로세스를 강제로 종료하면, 컨테이너가 비정상 상태로 인식되어 CrashLoopBackOff 됩니다.

- 프로세스 확인

  `ps aux`{{execute}}

- ENTRYPOINT인 pid:1를 강제 종료

  `trap "exit" SIGINT SIGTERM`{{execute}}

  `kill -s SIGINT 1`{{execute}}

강제종료를 하면 컨테이너가 재시작되어 쉘이 종료됩니다.
pod의 정보를 출력하면 restart 횟수가 1 증가한것을 확인할 수 있습니다.

`kubectl get pod`{{execute}}
