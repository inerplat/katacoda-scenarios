# 파일을 읽고 출력하는 SpringBoot API

아래에는 Absolute Path로부터 파일을 읽고, String으로 만들어서 출력하는 API가 준비되어 있습니다.
컨테이너 이미지에는 디버그를 위한 파일이 존재하지 않고, 컨테이너가 구동되기 전에 파일추가가 필요한 경우를 상정하겠습니다.

```java
@RestController
public class RootController {

    @GetMapping("/{fileName}")
    public String getFile(@PathVariable String fileName) throws FileNotFoundException {
        File file = new File("/file/" + fileName);
        FileInputStream fileInputStream = null;
        try {
            fileInputStream = new FileInputStream(file);
            InputStreamReader isr = new InputStreamReader(fileInputStream);
            StringBuilder sb = new StringBuilder();
            int c;
            while ((c = isr.read()) != -1) {
                sb.append((char) c);
            }
            return sb.toString();
        }
        catch (Exception e) {
            e.printStackTrace();
        }
        throw new FileNotFoundException();
    }
}
```

런타임에 사용되는 이미지입니다. Spring Boot의 내장톰캣을 이용해 실행됩니다

```dockerfile
FROM eclipse-temurin:11-jre
WORKDIR /run
COPY build/libs/insert-file.jar .

CMD ["java", "-jar", "insert-file.jar"]
```
