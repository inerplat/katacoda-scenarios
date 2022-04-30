# 실습환경 세팅

아래의 명령어를 실행하고, 실습환경이 구성될때까지 잠시 기다려주세요

`curl -sfL https://get.k3s.io | sh -`{{execute}}

`echo 'alias kubectl="k3s kubectl"' >> .bashrc && source .bashrc`{{execute}}

노드의 상태가 Ready가 되면 다음 스텝으로 넘어가 주세요

`kubectl get node`{{execute}}

```
$ kubectl get node
NAME     STATUS   ROLES                  AGE   VERSION
host01   Ready    control-plane,master   40s   v1.22.7+k3s1
```
