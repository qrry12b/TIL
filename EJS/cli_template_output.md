# CLI에서 템플릿 파일과 데이터 파일을 입력하고 출력 파일을 지정하기

EJS 는 Embedded JavaScript Template의 약자로 템플릿 모듈입니다   

기존 HTML 문법 사이에 javascript 코드를 작성 할 수 있으며 
express(Nodejs 웹프레임워크)에서 뷰 템플릿 엔진으로 많이 사용됩니다   

먼저 템플릿 파일을 작성합니다. 보통은 .ejs 확장자를 사용하지만 .html, .txt 등 다른 확장자로 작성할 수 있습니다.

- - - - -

**in.ejs**
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= title %></title>
</head>
<body>
    <%- content %>
</body>
</html>
```

여기서 <%= %> 태그는 템플릿에 데이터를 출력합니다. (단, HTML 이스케이프 처리를하기 때문에 데이터가 HTML 태그라면 & l t ; div & & g t ; 와 같이 출력됩니다/ MarkDown에서 이스케이프 문자를 작성해도 변환되어 공백을 추가했습니다)

<%- %> 태그는 위 태그와 거의 동일하지만 이스케이프 처리되지 않은 값을 템플릿에 출력한다는 차이점이 있습니다.   

- - - - -

데이터는 json 파일로 작성합니다. 

**data.json**
```
{
    "title" : "Hello, EJS",
    "content" : "<H1>Hello</H1>"
}
```

- - - - -

커멘드 라인에서 in file , data file , out filepath 를 지정하면 ejs 템플릿엔진으로 처리된 HTML 파일을 확인 할 수 있습니다.

**command**
```
ejs ./in.ejs -f ./data.json -o ./out.html
```

- - - - -

out.html을 확인해보면 ejs 템플릿 문법으로 작성된 부분이 데이터 문자열로 변환되었습니다

만약 ejs 를 찾을 수 없다면 **npm install ejs -g** 를 입력해 ejs 를 글로벌로 설치하면 됩니다

**out.html**
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello, EJS</title>
</head>
<body>
    <H1>Hello</H1>
</body>
</html>
```

### REF
* [ejs cli](https://github.com/mde/ejs/blob/main/README.md#cli)
* [ejs syntax](https://github.com/mde/ejs/blob/main/docs/syntax.md)