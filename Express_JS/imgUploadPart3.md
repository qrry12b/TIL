# Express.js 이미지 업로드 파트 3

파트 3 에서는 다중 파일 업로드 페이지를 작성합니다.

**path [ ./index.html ]**
```
<input type="file" name="file" id="file" multiple>
```

**path [ ./index.html ]**
```
function sendFile() {
    let files = document.getElementById('file').files;

    if(files.length === 0) {
        alert('파일을 선택해 주세요')
        return;
    }

    for(let idx = 0; idx < files.length; idx ++) {
        let name = files[idx].name;

        if(name === void 0 || name === null || name.length === 0) {
            alert('잘못된 파일 이름입니다');
            return;
        }

        let extIdx = name.lastIndexOf('.');
        let ext = '';
        if(0 <= extIdx) {
            ext = name.substring(extIdx).toLowerCase();
        }

        if( ['.png', '.jpg', '.jpeg', '.git'].includes(ext) === false) {
            alert('이미지 파일만 선택해주세요 (png, jpg, jpeg, git)');
            return;
        }
    }

    var formData = new FormData();
    var xmlHttp = new XMLHttpRequest();

    [].forEach.call(files, function(file) { formData.append('file', file); });

    xmlHttp.onreadystatechange = function() {
        if(this.readyState == XMLHttpRequest.DONE) {
            resetFile();
            if(this.status == 200 ) {
                let res = this.response;
                if(res !== void 0 && res !== null && res.result) {
                    alert('파일 업로드에 성공했습니다');
                    return;
                }    
            }
            alert('파일 업로드에 실패했습니다 code:'+this.status);
        }
    };

    xmlHttp.open("POST", "/upload");
    xmlHttp.responseType='json';
    xmlHttp.send(formData);
}
```

```
function sendFile() {
    let files = Array.from(document.getElementById('file').files);
    
    let imgArray = [];
    let otherArray = [];

    let file;
    while(file = files.shift()) {
        let name = file.name;

        if(name === void 0 || name === null || name.length === 0) {
            otherArray.push(file);
            continue;
        }

        let extIdx = name.lastIndexOf('.');
        let ext = '';

        if(0 <= extIdx) {
            ext = name.substring(extIdx).toLowerCase();
            console.log(ext);
        }

        if(['.png', '.jpg', '.jpeg', '.git'].includes(ext) === false) {
            otherArray.push(file);
            continue;
        }

        imgArray.push(file);
    }

    files.push(...imgArray);
    imgArray.length = 0;

    if(otherArray.length !== 0) {
        alert('이미지가 아닌 파일이 ' + otherArray.length + '개 있습니다');
    }

    if(files.length === 0) {
        alert('업로드 가능한 이미지가 없습니다');
        return;
    }

    var formData = new FormData();
    var xmlHttp = new XMLHttpRequest();

    files.forEach(file => { formData.append('file', file); });

    xmlHttp.onreadystatechange = function() {
        if(this.readyState == XMLHttpRequest.DONE) {
            resetFile();
            if(this.status == 200 ) {
                let res = this.response;
                if(res !== void 0 && res !== null && res.result) {
                    alert('파일 업로드에 성공했습니다');
                    return;
                }    
            }
            alert('파일 업로드에 실패했습니다 code:'+this.status);
        }
    };

    xmlHttp.open("POST", "/upload");
    xmlHttp.responseType='json';
    xmlHttp.send(formData);
}
```

/#TODO#/