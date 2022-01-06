# Express.js 이미지 업로드 파트 1

파트 1 에서는 express, multer 모듈을 사용한 이미지 파일 업로드를 작성합니다.

**install package**
```
npm i dotenv express multer
```

express는 웹 프레임워크로 Nodejs 기본 모듈 http를 사용하는것보다 빠르고 쉽게 서버를 구성할 수 있습니다

multer는 Multipart/form-data 를 다루기 위한 미들웨어이며 해당 타입을 요청을 받았을때만 미들웨어가 동작합니다

**path [ ./index.js ]**
```
require('dotenv').config();
var env = process.env;

const express = require('express'), app = express()
const http  = require('http'), path = require('path'), fs = require('fs');

const uploadPath = path.join(__dirname, 'uploads');
if(!fs.existsSync(uploadPath)) {
    fs.mkdirSync(uploadPath);
}

const multer = require('multer');
const storage = multer.diskStorage({
    destination: function (req, file, cb) {
        cb(null, uploadPath)
    },
    filename: function (req, file, cb) {
        let ext = path.extname(file.originalname);
        cb(null, 'f'+Math.floor(Math.random()*999+1000)+'_'+Date.now()+ext)
    }
})
const upload = multer({ 
    storage: storage,
    fileFilter : (req,file, callback) => {
        let ext = path.extname(file.originalname).toLowerCase();
        if(ext === '.png' || ext === '.jpg' || ext === '.jpeg' || ext === '.git') {
            return callback(null, true);
        }else{
            return callback(null, false);
        }
    }
}) 

const port = env.PORT || 9982;

app.get('/favicon.ico', (req, res) => res.sendStatus(204));

app.use(express.json(), function(error, req, res, next) {
    if (error instanceof SyntaxError) {
        res.send({code : -1, err : 'JSON Parse SyntaxError'});
    } else {
      next();
    }
});

app.get('/', (req,res)=>{
    res.sendFile(__dirname + '/index.html');
});

app.post('/upload', upload.array('file'), (req,res)=>{
    console.log(req.file , req.files);
    res.send({ result : true });
});

app.use(function(req,res) {
    console.error('HTTP 404 :: ', req.originalUrl);
    res.status(404).send('HTTP 404');
    return;
});

app.use(function(err,req,res,next) {
    console.error('HTTP 500 :: ', req.originalUrl);
    console.error(err.stack);
    res.status(500).send('HTTP 500');
    return;
});

http.createServer(app).listen(port, function() {
    console.log('server run port :: '+port);
});

process.on('uncaughtException', function (error) {
    console.log('uncaughtException :: ',error);
});

process.on('exit', function (code) {
    console.log('process exit code :: '+code);
});

process.on('SIGINT', function () {
    process.exit(0);
});

process.on('SIGTERM', function () {
    process.exit(0);
});
```

개인적으로 자주 구성하는 express 코드를 복사해두고 템플릿처럼 사용하고 있기 때문에 최소한의 업로드 구현시에는 필요 없는 코드도 포함되어 있습니다.   

최상위의 dotenv 모듈은 별도의 파라미터가 없을 경우 루트의 .env 파일을 읽어 process.env에 읽은 값을 설정합니다.   

config 값을 가진 js를 사용하거나 다른 방법으로 환경변수를 설정할 경우 필요 없는 코드입니다.

path, fs는 파일이나 디렉터리가 존재하는지 확인하거나 생성,읽기,쓰기 등의 작업에 사용하는 Nodejs 기본모듈 입니다.

http.createServer 없이도 app.listen으로 바로 서버를 실행할 수 있지만 서버가 이후 다른 모듈 (예 socket.io)과 같이 사용될 수 있기 때문에 http 서버를 생성해 사용합니다.

multer.diskStorage 에 전달하는 destination는 요청에 따른 업로드 경로를, filename에서는 요청에 따른 파일 이름을 지정할 수 있습니다.

multer.diskStorage는 multer모듈에 storage 라는 이름으로 전달합니다.

fileFilter는 파일 저장여부를 필터링 할 수 있으며 실제 파일을 저장할지는 callback의 두번째 인자로 전달할 수 있습니다.

만약 사용자의 로그인/등급 권한 체크가 필요한 경우 multer의 fileFilter에서 확인하는것이 아닌 multer를 호출하기 이전에 검사하도록 합니다. (해당 함수의 목적은 파일의 필터이며 업로드 로직과 인증 로직은 분리되어야 합니다)

만약 fileFilter의 callback에서 두번째 인자에 false를 반환할 경우 해당 파일은 req.file, req.files에서 제외됩니다.

또한 첫번째 인자에 null 이 아닌 Error를 생성해 반환하면 multer는 에러를 express에 위임합니다.   
(HTTP 5XX 반환, 오류 페이지를 보여줄 때)

단, express가 아닌 multer 로 부터 에러를 캐치하고 싶다면 직접 미들웨어 함수를 호출 할 수 있습니다.

**example**
```
//multer
fileFilter : (req,file, callback) => {
    let ext = path.extname(file.originalname).toLowerCase();
    if(ext === '.png' || ext === '.jpg' || ext === '.jpeg' || ext === '.git') {
        return callback(null, true);
    }else{
        return callback(new Error('a'), false);
    }
}

//express
app.post('/upload', /*upload.array('file'),*/ (req,res)=>{
    upload.array('file')(req,res,(err)=>{
        if(err) {
            console.log('catch err');
            res.send({ result : false });
        } else {
            console.log(req.file, req.files);
            res.send({ result : true });
        }
    });    
});
```

이 경우 비동기로 콜백이 실행되며 err가 전달되는지에 따라 다른 처리나 응답을 할 수 있습니다.

.single 은 인자에 명시된 이름의 단수 파일을 전달받습니다.   
.single로 받은 파일의 정보는 req.file 에 전달됩니다.

.array 는 인자에 명시된 이름의 파일 전부를 배열 형태로 전달받습니다.   
.array의 선택적인 두번째 인자 maxCount를 명시하면 값 이상으로 파일이 업로드 될 경우 에러를 출력할 수 있습니다.   
.array로 받은 파일의 정보는 req.files에 저장됩니다.

.fields 는 인자에 명시된 여러 파일을 전달 받을 수 있으며 req.files에 저장됩니다.

.none 은 multipart/form-data 요청에 대해 미들웨어가 처리하도록 하지만 텍스트 필드만 허용합니다.   

텍스트 필드가 있는 경우 req.body에 포함됩니다.

**path [ ./index.html ]**
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Upload Image</title>
</head>
<body>
    
</body>
</html>
```

업로드 페이지는 다음 파트에서 작성하도록 하며 404 예외가 발생하지 않도록 빈 페이지만 생성합니다.   

웹페이지가 작성되지 않은 상태에서 업로드 요청에 대한 동작은 Postman 등의 툴로 확인할 수 있습니다.

[Express.js 이미지 업로드 파트 2](./imgUploadPart2.md)

### REF
* [multer github doc-ko](https://github.com/expressjs/multer/blob/master/doc/README-ko.md)