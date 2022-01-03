# &lt;% %&gt; 태그로 흐름제어 및 include 사용하기

**in.ejs**
```
<tbody>
    <% if(list == void 0 || list == null || list.length == 0) { %>
        <tr>
            <td colspan="3">리스트가 비었습니다</td>
        </tr>
    <% } else { %>
        <% list.forEach(function(item){ %>
            <%- include('item', { item : item } ); %>
        <% }); %>
    <% } %>
</tbody>
```

만약 JSP를 다뤄봤다면 EJS 스크립트릿(Scriptlet), 표현식(Expession)을 쉽게 이해할 수 있습니다.   

&lt;% %&gt; 템플릿 문법을 사용해서 HTML 사이에 자바스크립트로 흐름 제어를 할 수 있습니다.   

&lt;% %&gt; 템플릿 문법으로 데이터의 출력은 할 수 없지만 스크립트릿 내부에서 Javascript 코드를 사용해 흐름을 제어할 수 있습니다.   

위 예시에서는 배열이 없거나 비어있다면 "리스트가 비었습니다" 를 출력하며 배열이 있을 경우 출력하도록 작성되어 있습니다.   

include 는 다른 ejs 템플릿 파일을 내부에 포함할 수 있습니다. 위 예시에서는 테이블의 tr 부분부터 별도의 item.ejs 파일로 분리하여 배열의 데이터를 순환하며 item 이라는 이름으로 전달하고 있습니다.   

**item.ejs**
```
<tr>
    <td><%- item.title %></td>
    <td><%- item.author %></td>
    <td><%- item.dateFormat %></td>
</tr>
```

item.ejs 에서는 item. ~ 과 같이 전달받은 배열 아이템의 키값으로 데이터를 출력합니다.   

**data.json**
```
{
    "list" : [
        {
            "title" : "Hello, EJS",
            "author" : "User1",
            "dateFormat" : "2022-01-03"
        },
        {
            "title" : "EJS Study",
            "author" : "User2",
            "dateFormat" : "2022-01-03"
        },
        {
            "title" : "EJS Test",
            "author" : "User3",
            "dateFormat" : "2022-01-03"
        }
    ]
}
```

data.json을 작성하고 빈 배열을 만들어 보거나 배열을 채우거나 하면서 상황에 따라 다른 출력을 확인할 수 있습니다.