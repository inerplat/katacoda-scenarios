# 컨테이너의 실행 명령어를 대체

파드를 배포할 때 Dockerfile에 작성된 컨테이너의 명령어를 대체할 수 있습니다.

- 기존에 배포한 파드를 삭제

  `kubectl delete pod interpreted-lang --wait=false`{{execute}}

- command / args를 재작성

  <pre class="file" data-filename="pod2.yaml" data-target="prepend">
  apiVersion: v1
  kind: Pod
  metadata:
    name: interpreted-lang2
    labels:
      app: intrepreted-lang2
  spec:
    containers:
      - name: fastapi
        image: inerplat/container-debug-example:interpreted-lang
        ports:
          - containerPort: 80
        command:
          - /bin/sh
        args:
          - -c
          - tail -f /dev/null
  </pre>

위와 같이 작성하면 실행된 쉘이 /dev/null을 테일링하여 종료되지 않습니다.

이제 파드내의 코드를 수정하고 실행하여 디버깅을 수행할 수 있습니다.

- 명령어를 새로 작성한 파드 배포

  `kubectl apply -f pod2.yaml`{{execute}}

- 컨테이너의 쉘에 접근

  `kubectl exec -it interpreted-lang2 -- /bin/bash`{{execute}}

- 컨테이너 내부의 소스코드 수정

  `sed -i -E "s/Hell World/Hello World/g" index.py`{{execute}}

- 어플리케이션을 백그라운드에서 실행

  `nohup uvicorn index:app --host 0.0.0.0 --port 80 1>debug.log 2>&1 &`{{execute}}

- 컨테이너 쉘 종료

  `exit`{{execute}}

- 어플리케이션 응답 확인

  `POD_IP=$(kubectl get pod interpreted-lang2 -o=jsonpath="{.status.podIP}")`{{execute}}
  
  `curl $POD_IP`{{execute}}


