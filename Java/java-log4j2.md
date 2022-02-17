# Java기반 로깅 유틸리티 - log4j 2

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.29)   

로깅 유틸리티 Log4J 2를 정리한다. Log4J 1.2보다 성능이 향상되었으며 취약점이 발견되었기 때문에 Log4J 2를 권장하며 라이센스는 Apache License, Version 2.0 를 따르고 있다.

간단하게 이클립스에서 Java Project를 생성한 다음 Packaging을 jar로 해서 Maven Project로 변경했다. 그다음 Java Build Path 에 기본적으로 추가되어 있는 src를 제거 후 src/main/java 와 src/main/resources 를 추가했다. 추가하는 방법은 프로젝트를 우클릭 후 Build Path -> New Source Folder 에서 추가하면 된다.

기존 src를 지우고 추가했다면 pom.xml 에서 sourceDirectory 를 src/main/java로 수정하고 LOG4J 2의 dependency 를 추가한다.

log4j2.xml 파일의 작성 방법은 #6 를 참고할 수 있다.

Java 코드는 간단하게 작성했다.

```
public class TestClass {
 private static Logger Logger = LogManager.getLogger(TestClass.class);
 public static void main(String[] args) {
   Logger.debug("Debug");
   Logger.info("Info");
   Logger.error("Error");
 }
}
```

### REF
* [log4j2 #1](https://logging.apache.org/log4j/2.x/)
* [log4j2 #2](https://logging.apache.org/log4j/2.x/maven-artifacts.html#Using_Log4j_in_your_Apache_Maven_build)
* [log4j2 #3](https://logging.apache.org/log4j/2.x/manual/configuration.html)
* [howtodoinjava](https://howtodoinjava.com/log4j2/log4j-2-xml-configuration-example/)
* [wikidoc](https://wikidocs.net/18340)
* [stackoverflow](https://stackoverflow.com/questions/36907204/how-to-create-a-src-main-resources-directory)