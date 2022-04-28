# 실습환경 세팅

실습환경이 구성될때까지 잠시 기다려주세요

`curl -sfL https://get.k3s.io | sh -`{{execute}}

`kubectl get node`{{execute}}

```
$ kubectl get node
NAME     STATUS   ROLES                  AGE   VERSION
host01   Ready    control-plane,master   40s   v1.22.7+k3s1
```
