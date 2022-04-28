# 컨테이너의 실행 명령어를 대체

파드를 배포할 때 컨테이너 이미지에 작성된 컨테이너의 명령어를 대체할 수 있습니다.

- 기존에 배포한 파드를 삭제

  `kubectl delete pod python-missed-package`{{execute}}

- command / args를 재작성

  <pre class="file" data-filename="pod2.yaml" data-target="prepend">
  apiVersion: v1
  kind: Pod
  metadata:
    name: python-missed-package
    labels:
      app: python-missed-package
  spec:
    containers:
      - name: fastapi
        image: inerplat/container-debug-example:python-missed-package
        ports:
          - containerPort: 80
        command:
          - /bin/sh
        args:
          - -c
          - tail -f /dev/null
  </pre>

위와 같이 작성하면 실행된 쉘이 /dev/null 파일을 테일링하여 종료되지 않습니다.

이제 파드내의 코드를 수정하고 실행하여 디버깅을 수행할 수 있습니다.

- 명령어를 새로 작성한 파드 배포

  `kubectl apply -f pod2.yaml`{{execute}}

