# 간단한 FastAPI 어플리케이션 배포


아래와 같이 텍스트를 반환하는 FastAPI 어플리케이션과, 컨테이너 이미지가 준비되어 있습니다.

- FastAPI

  ```python
  from fastapi import FastAPI

  app = FastAPI()


  @app.get("/")
  def root_controller():
      return "Hell World"
  ```
- Dockerfile

  ```Dockerfile
  FROM python:3.10
  WORKDIR /app

  RUN pip install fastapi uvicorn[standard]

  COPY index.py .

  CMD ["uvicorn", "index:app", "--host", "0.0.0.0", "--port", "80"]
  ```

컨테이너 이미지를 파드로 만들어서 배포합니다.

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