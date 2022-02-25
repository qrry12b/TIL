# D3 (Data-Driven Documents)
데이터 시각화 프레임워크로 잘 알려져 있고 JQuery 처럼 Sizzle Selector를 사용하는것으로 알고 있다.

D3의 경우 Selection 뒤에 .data()로 참조할 데이터를 연결한다.   

Selection에 선택된 Element가 데이터보다 적을수도 있고 많을 수도 있다.

.enter() 메소드로 부족한 갯수만큼 각 데이터에 대한 자리 표시자 노트를 반환하고   
.append() 메소드를 체이닝해서 부족한 갯수만큼 태그를 추가할 수 있다.   

만약 데이터보다 Element가 많을 경우 .exit() 메소드로 불필요한 DOM 요소를 선택할 수 있으며   
.exit() 메소드를 체이닝해서 남는 갯수만큼 태그를 제거할 수 있다.   

D3는 데이터 시각화 프레임워크로 잘 알려져 있는 만큼 다양한 시각화 목적에 따라 사용될 수 있는데   
웹에서 많이 사용되는 차트를 만들기 위한 라이브러리가 내부에서 d3를 사용하는 경우도 볼 수 있다. (예, billboard.js)   

또한 .enter(), .exit() 메소드를 각자 호출해 사용하는것이 아닌 .join() 메소드를 통해 쉽게 새로운 요소를 생성하고, 필요없는 요소를 삭제할 수 있다.

만약 이미 있는 요소를 선택한 Selection과 .enter().append()를 통해 새로운 요소를 만든 선택자를   
중복된 코드를 작성하거나 SelectQuery를 다시 할 필요 없이 .merge() 메소드를 통해 기존 요소와 새로 만든 요소를 하나의 Selection으로 합칠 수 있다.

[REF](https://github.com/d3/d3-selection/blob/v3.0.0/README.md)