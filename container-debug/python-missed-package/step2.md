# 간단한 FastAPI 어플리케이션 배포


아래와 같이 Numpy를 통해 생성된 행렬을 List로 반환하는 FastAPI 어플리케이션과, 컨테이너 이미지가 준비되어 있습니다.

- FastAPI

```python
from fastapi import FastAPI, Response, status
import numpy as np
app = FastAPI()


@app.get("/", status_code=200)
def root_controller(row: int, col: int):
    return np.random.rand(row, col).tolist()

```

도커파일에서는 Numpy 패키지의 설치가 누락되었습니다.

- Dockerfile

```Dockerfile
FROM python:3.10
WORKDIR /app

RUN pip install fastapi uvicorn[standard]

COPY index.py .

CMD ["uvicorn", "index:app", "--host", "0.0.0.0", "--port", "80"]
```

위와 같이 작성한 어플리케이션을 컨테이너 이미지로 만들어서 파드에 배포합니다.

<pre class="file" data-filename="pod.yaml" data-target="prepend">
apiVersion: v1
kind: Pod
metadata:
  name: interpreted-lang
  labels:
    app: intrepreted-lang
spec:
  containers:
    - name: fastapi
      image: inerplat/container-debug-example:interpreted-lang
      ports:
        - containerPort: 80
</pre>

`kubectl apply -f pod.yaml`{{execute}}