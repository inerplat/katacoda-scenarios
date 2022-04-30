# 방법 1. ConfigMap을 이용하는 방법

파일의 용량이 1MB를 넘지 않고, 일반적인 텍스트로 표현된다면 Configmap을 이용해 파일을 마운트할 수 있습니다.

아래와 같이 `insert-file1.txt` configmap을 작성하고 클러스터에 배포할 수 있습니다

```
cat << EOF | kubectl apply -f -
kind: ConfigMap
apiVersion: v1
metadata:
  name: insert-file1-cm
data:
  insert-file1.txt: |
    Hello World!
    If you're reading this
    EHAGHKDCI 
EOF    
```{{execute}}

작성한 Configmap은 아래와 같이 파일로 마운트할 수 있습니다.

```
cat << EOF | kubectl apply -f -
kind: Deployment
apiVersion: apps/v1
metadata:
  name: container-debug-example-insert-file
  labels:
    app: insert-file
spec:
  selector:
    matchLabels:
      app: insert-file
  template:
    metadata:
      labels:
        app: insert-file
    spec:
      containers:
        - name: app
          image: inerplat/container-debug-example:insert-file
          ports:
            - containerPort: 8080
          volumeMounts:
            - name: debug-volume
              mountPath: /file/insert-file1.txt
              subPath: insert-file1.txt
      volumes:
        - name: debug-volume
          configMap:
            name: insert-file1-cm
EOF
```{{execute}}

`POD_IP=$(kubectl get pod -l app=insert-file -o jsonpath="{.items[0].status.podIP}")`{{execute}}

`curl "http://$POD_IP/insert-file1.txt"`{{execute}}
