# Express 뷰 템플릿 엔진으로 EJS 사용하기

```
npm i express ejs
```

```
const express = require('express');
const app = express();
const path = require('path');

app.set('views', [ 
    path.join(__dirname, 'views'), 
    path.join(__dirname, 'html_views') 
]);

app.set('view engine', 'ejs');
app.engine('html', require('ejs').renderFile);

app.get(['/' , '/index'], (req,res)=>{
    // views/index.ejs
    res.render('index');
});

app.get('/html', (req,res)=>{
    // html_views/index.html
    res.render('index.html', {content : 'CONTENT'});
});

app.use((req,res,next)=>{
    res.status(404).send('HTTP 404');
});

app.use((err,req,res,next)=>{
    res.status(500).send('HTTP 500');
});

app.listen(9982, ()=>{
    console.log('listen port 9982');
});
```

### REF
* [ejs wiki](https://github.com/mde/ejs/wiki/Using-EJS-with-Express)
* [expressjs api app.set](http://expressjs.com/en/api.html#app.set)
* [expressjs api app.engine](http://expressjs.com/en/api.html#app.engine)