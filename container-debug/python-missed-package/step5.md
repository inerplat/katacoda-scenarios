# 컨테이너의 쉘을 이용한 디버그

컨테이너 이미지에 쉘이 포함되어 있다면, 쉘을 이용해 컨테이너 내부에 접근해 디버깅을 수행할 수 있습니다.

- 컨테이너의 내부의 쉘을 실행

  `kubectl exec -it python-missed-package -- /bin/sh`{{execute}}

- 누락된 패키지 설치

  `pip install numpy`{{execute}}

- 어플리케이션을 백그라운드에서 실행

  `nohup uvicorn index:app --host 0.0.0.0 --port 80 1>debug.log 2>&1 &`{{execute}}

- 컨테이너 내부에서 어플리케이션 응답 확인

  `curl localhost/?row=2&col=3`{{execute}}

- 컨테이너 쉘 종료

  `exit`{{execute}}

- 컨테이너 외부에서 어플리케이션 응답 확인

  `POD_IP=$(kubectl get pod python-missed-package -o=jsonpath="{.status.podIP}")`{{execute}}
  
  `curl $POD_IP/?row=2&col=3`{{execute}}
