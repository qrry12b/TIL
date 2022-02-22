## 이클립스 Workspace JDK 기본 버전 및 경로 설정
Window > Preferences > Java 

\> Compiler > JDK Compliance > Compiler compliance level : 1.8

\> Installed JREs > Add > Standard VM > Directory > (JDK 1.8 설치 경로) 폴더 선택 > Finish
\> 리스트에서 JRE 1.8 체크 > Apply > Apply and Close

## 이클립스 Workspace 기본 인코딩 설정
Window > Preferences > Workspace
\> Text file encoding > Other : UTF-8 > Apply > Apply and Close

## 이클립스 SVN Checkout
Window > Show View > Other
\> SVN > SVN Repositories
\> New Repository Location > (SVN 정보 입력) > Save authentication (could trigger secure storage login) 체크 > Finish
\> 추가된 Repository 우클릭 > Checkout

## 이클립스 Progress JPA Project Change Event Handler (waiting) 반복
- Window > Preferences > Maven > Java EE Integration > JPA Configurator 체크 해제 > Apply > Apply and Close
- Window > Preferences > Validation > Suspend all validators 체크 해제 > Apply > Apply and Close
- Marketplace STS 관련 업데이트

## 프로젝트가 깨질 경우 (프로젝트 우클릭 후 Properties)
- Resource > Text file encoding > UTF-8
- Java Build Path 에서 Libraries에 찾을 수 없는 항목들 재설정, Order and Export에서 라이브러리 참조순서 맞춤
- Java Compiler에서 설정된 Compiler compliance level과 Workspace 에 설정한 JDK Compilance 가 다를 경우 프로젝트에 맞춰서 Java Build Path 재설정 및 선택
- Project Facets > Java 버전 맞추기, Dynamic Web Module 버전 맞추기, Runtimes에서 버전에 맞는 서버 선택하기

# 프로젝트가 깨질 경우
pom.xml 에서 build / plugins / plugin / org.apache.maven.plugins#maven-compiler-plugin 에 설정된 configuration 버전이 현재 JAVA 버전과 일치하는지 확인