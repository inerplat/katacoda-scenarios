# 배포된 파드를 확인

아래의 명령어로 파드의 상태를 확인할 수 있습니다.

`kubectl get pod`{{execute}}

```
$ kubectl get pod
NAME                    READY   STATUS    RESTARTS   AGE
python-missed-package   1/1     Running   0          7m17s
```

`STATUS: Running`이 확인되면 POD_IP로 `GET` 요청을 보내 응답을 확인할 수 있습니다.


`POD_IP=$(kubectl get pod python-missed-package -o=jsonpath="{.status.podIP}")`{{execute}}

`curl $POD_IP/?row=2&col=3`{{execute}}
