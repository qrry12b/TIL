# Eclipse JPA Project Change Event Handler (waiting) 반복 해결

Eclipse 에서 프로젝트를 열었을때 하단의 Progress 에 JPA Project Change Event Handler (waiting)로 채워지며  
매우 버벅거리다가 응답없음으로 종료되는 경우 아래와 같은 설정을 체크 해제했습니다.

> Window > Preferences > Maven > Java EE Integration > JPA Configurator 체크 해제   
> Window > Preferences > Validation > Suspend all validators 체크 해제