# 배포된 파드를 확인

아래의 명령어로 파드의 상태를 확인할 수 있습니다.

`kubectl get pod`{{execute}}

```
$ kubectl get pod
NAME                    READY   STATUS             RESTARTS   AGE
python-missed-package   0/1     CrashLoopBackOff   6          5m48s
```

파드내의 컨테이너가 재시작을 반복해도 실행되지 못하면, `CrashLoopBackOff` 상태가 됩니다.

문제가 생긴 파드의 로그는 `kubectl logs [pod이름]` 을 통해 확인할 수 있습니다.

`kubectl logs python-missed-package`{{execute}}

```
Traceback (most recent call last):
  File "/usr/local/bin/uvicorn", line 8, in <module>
    sys.exit(main())
    ......
  File "<frozen importlib._bootstrap_external>", line 883, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/app/./index.py", line 2, in <module>
    import numpy as np
ModuleNotFoundError: No module named 'numpy'
```

이번 예제는 로그를 통해서도 쉽게 원인을 파악할 수 있습니다.

하지만 그 외에 복합적인 오류가 발생하거나 로그를 통해 파악하기 어려운 경우에, 매번 컨테이너 이미지를 빌드하며 디버그를 수행하면 해결까지 오랜 시간이 소요될 수있습니다.
