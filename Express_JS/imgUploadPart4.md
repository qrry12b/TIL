# Express.js 이미지 업로드 파트 #

파트 4 에서는 sharp 모듈을 사용한 이미지 리사이징을 다룹니다.

**install package**
```
npm i sharp
```

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

**index.js**
```
app.post('/upload', upload.array('file'), sharpResize, (req,res)=>{
    console.log(req.file , req.files);
    res.send({ result : true });
});
```

**console log**
```
path: 'uploads\\f1293_1641822126924.PNG',
size: 60584,
thum: 'uploads\\f1293_1641822126924_thum.jpeg',
thumSize: 16801
```

다음 파트에서는 업로드할 이미지 미리보기를 다룹니다.

### REF
* (Github sharp)[https://github.com/lovell/sharp]
* (Sharp)[https://sharp.pixelplumbing.com]