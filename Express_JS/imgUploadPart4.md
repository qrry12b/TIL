# Express.js 이미지 업로드 파트 4

파트 4 에서는 sharp 모듈을 사용한 이미지 리사이징을 다룹니다.

**install package**
```
npm i sharp
```
sharp 패키지를 설치합니다

**index.js**
```
const sharpResize = (req, res, next) => {
    const files = [];
    if(req.file !== void 0 && req.file !== null) {
        files.push(req.file);
    }
    if(req.files !== void 0 && req.files !== null && req.files.length !== 0) {
        files.push(...req.files);
    }
    
    (async (files)=>{
        for(let i=0; i<files.length; i++) {
            let file = files[i];
            let thumPath = path.resolve(file.path, '../', path.basename(file.path, path.extname(file.path))+'_thum'+'.jpeg')
            try{
                let height = 320;
                await sharp(file.path).metadata().then(( data ) => height = Math.min(data.height, height) );
                await sharp(file.path).resize({ height: height }).toFormat('jpeg').toFile(thumPath);
                file.thum = thumPath;
                file.thumSize = fs.statSync(thumPath).size;
            }catch(err) {
                file.thumError = err;
                console.err(err);
            }
        }

        next();
    })(files);
};
```
방법은 다양하지만 여기서는 미들웨어로 추가합니다.   

미들웨어로 추가할때는 반드시 request, response, next 인자를 받아야 하며 next를 호출해야 다음 미들웨어로 넘어갑니다.   

가장 먼저 미들웨어로 들어오면 req.file 혹은 req.files에 파일 정보가 있는지 확인하고 files 배열에 추가합니다.   

...Array는 이전 파트에서 설명했으므로 생략합니다.   

sharp 는 비동기로 Promise를 반환하기 때문에 동기방식으로 실행하려면 async 함수로 만들어야 하지만   

IIFE로 async function 표현식을 사용할 수 있기 때문에 미들웨어 함수에 async 키워드를 사용하지 않고 내부에서 async function을 작성합니다.   

배열을 순환하면서 file의 메타 정보를 읽어 320px 혹은 이보다 작은 원본 이미지의 height 크기를 height로 해서 리사이징 합니다.   

리사이징된 정보는 thum, thumSize 그리고 리사이징시 발생한 예외는 thumError에 추가되도록 작성했습니다.   

모든 파일 배열을 순환하면 다음 미들웨어를 실행합니다.

**index.js**
```
app.post('/upload', upload.array('file'), sharpResize, (req,res)=>{
    console.log(req.file , req.files);
    res.send({ result : true });
});
```

미들웨어를 작성했다면 기존 요청 사이에 추가합니다.   

주의할점은 multer 뒤에 위치해야 업로드된 파일 정보를 가져올 수 있으며 요청에 대한 처리는 리사이징 미들웨어 뒤에 있어야 리사이징된 파일 정보를 가져올 수 있습니다.   

**console log**
```
path: 'uploads\\f1293_1641822126924.PNG',
size: 60584,
thum: 'uploads\\f1293_1641822126924_thum.jpeg',
thumSize: 16801
```

로그를 확인해보면 file Object에 thum, thumSize 정보가 추가되어 있습니다.   

다음 파트에서는 업로드할 이미지 미리보기를 다룹니다.

### REF
* [Github sharp](https://github.com/lovell/sharp)
* [Sharp](https://sharp.pixelplumbing.com)
* [MDN Web Docs - Spread_syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
* [MDN Web Docs - async_function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/async_function)