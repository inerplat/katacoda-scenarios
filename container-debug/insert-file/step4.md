# 방법 2. InitContainer를 이용하는 방법

파일의 용량이 크거나, 복사 이후에 다른 추가 작업이 필요하다면 InitContainer를 사용할 수 있습니다

InitContainer는 Container보다 먼저 동작하며, Container가 동작하려면 InitContainer가 정상종료되어야 합니다.
(InitContainer가 여러개가 있다면 1개씩 순차적으로 동작합니다)

위의 특성을 활용하면, 개발자가 원하는 시점까지 어플리케이션 컨테이너 구동을 유예하고 사전 작업을 수행할 수 있습니다.

아래의 배포에서는 InitContainer와 어플리케이션 Containers가 empty-dir의 `/file` 경로를 공유하도록 작성되었습니다
InitContainer는 `/tmp/die` 파일이 생성되었는지 5초마다 체크를 하고, 파일이 존재하면 종료됩니다,

```
cat << EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: container-debug-example-insert-file2
  labels:
    app: insert-file2
spec:
  selector:
    matchLabels:
      app: insert-file2
  template:
    metadata:
      labels:
        app: insert-file2
    spec:
      initContainers:
        - name: insert
          image: busybox
          command:
            - /bin/sh
            - -c
            - "while [ ! -f /tmp/die ]; do sleep 5; done"
          volumeMounts:
            - name: debug-volume2
              mountPath: /file
              readOnly: false
      containers:
        - name: app
          image: inerplat/container-debug-example:insert-file
          ports:
            - containerPort: 8080
          volumeMounts:
            - name: debug-volume2
              mountPath: /file
              readOnly: false
      volumes:
        - name: debug-volume2
          emptyDir: { }
EOF
```{{execute}}

이제 동작중인 파드의 쉘에 접근하여 파일을 생성할 수 있습니다.

> 혹은 `kubectl cp` 명령어를 통해 로컬파일을 컨테이너로 옮길 수 있습니다)
>
> ex. 로컬의 debugger.bin 파일을 'insert' 컨테이너에 옮기고 싶은 경우
>
> kubectl cp -c insert debugger.bin $POD2_NAME:file/debugger.bin

```
POD2_NAME=$(kubectl get pod -l app=insert-file2 -o jsonpath="{.items[0].metadata.name}")
kubectl exec -it $POD2_NAME -c insert -- /bin/sh
```{{execute}}


```
cat > /file/insert-file2.txt << EOF
Did you practice this and get off work faster?
Yeah that's it
EOF
```{{execute}}

사전작업이 완료되었다면 `/tmp/die` 파일을 생성해줍니다

`touch /tmp/die`{{execute}}

5초정도 기다리면 initcontainer가 자동 종료됩니다.
이제 컨테이너 외부에서 API를 호출해 봅시다.

파드에 IP가 할당될때까지 잠시 기다립니다.

```
POD2_IP=$(kubectl get pod -l app=insert-file -o jsonpath="{.items[0].status.podIP}")
echo $POD2_IP
```{{execute}}


할당된 IP를 이용해 API를 호출합니다.

```curl "http://$POD2_IP:8080/insert-file2.txt"```{{execute}}