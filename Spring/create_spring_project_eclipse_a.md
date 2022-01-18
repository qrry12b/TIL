# Eclipse에서 Spring 프로젝트 생성하기-A

### Marketplace에서 Spring Tools 3 (Standalone Edition) or Spring Tools 4 (aka Spring Tool Suite 4) 를 설치하면 더 쉽게 구성 가능합니다.

이클립스에서 New > Dynamic Web Project 를 클릭합니다.
Target runtime : tomcat 8.n.n
Dynamic web module version : 3.1
Configuration : Default Configuration for apache-tomcat-8.n.n

Next > Next 클릭후 Generate web.xml deployment descriptor를 선택하고 Finish를 클릭합니다.

- - - - -

만약 프로젝트 생성시 Generate web.xml deployment descriptor 선택을 못했다면 프로젝트 우클릭 > Java EE Tools > Generate Deployment Descriptor Stub 을 선택하면 됩니다.

- - - - -

만약 web.xml 에서 \<web-app 줄에 빨간 경고가 출력된다면 네임스페이스가 다른 버전으로 되어 있는지 확인합니다.   

```
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
```

- - - - -

프로젝트 우클릭 > Configure > Convert to Maven Project를 클릭합니다.

- - - - -

프로젝트 우클릭 > Properties를 클릭 후 (shortcut Alt+Enter)   

Resource 에서는 Text file encoding 을 UTF-8로 설정합니다.   
Java Build Path 에서는 JRE와 Server Runtime을 맞춰줍니다.   

이 예시에서는 Project Facets 항목을 Dynamic Web Module 3.1, Java 1.8 을 선택하며 우측에 Runtimes 에 apache-tomcat 이 선택되어 있는지 확인합니다.

수정을 완료했다면 Apply and Close를 클릭합니다.

- - - - -

만약 클래스를 찾을 수 없다는 에외가 발생하면 프로젝트 우클릭 > Maven > Update Project 을 먼저 하고   

프로젝트 우클릭 > Properties를 클릭 후 Deployment Assembly에 Maven Dependencies 가 있는지 확인하며 없을 경우 Add > Java Build Path Entries > Maven Dependencies를 추가합니다.

- - - - -

src/main/webapp/index.jsp 파일을 생성 후   
Tomcat Server에 프로젝트를 추가 후 실행합니다.

루트(/) 혹은 /프로젝트명 으로 접근했을때 404가 아닌 빈 페이지가 출력되는지 확인합니다.   

만약 루트(localhost:8080)로 프로젝트를 열고 싶다면 톰캣서버의 server.xml 하단에 추가되어 있는 프로젝트의 path 를 "/" 으로 수정합니다.   

- - - - -

//TODO// 여기서 부터 계속 작성됩니다.