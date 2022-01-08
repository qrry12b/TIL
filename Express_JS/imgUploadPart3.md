# Express.js 이미지 업로드 파트 3

파트 3 에서는 다중 파일 업로드 페이지를 작성합니다.

**path [ ./index.html ]**
```
<input type="file" name="file" id="file" onchange="selectFile(this);"  multiple>
```

input file 태그에 multiple(bool)을 추가하면 한번에 여러 파일을 선택하는것이 가능합니다.

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

        if( ['.png', '.jpg', '.jpeg', '.gif'].includes(ext) === false) {
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

**path [ ./index.html ]**
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

        if(['.png', '.jpg', '.jpeg', '.gif'].includes(ext) === false) {
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

**path [ ./index.html ]**
```
<body>
    <input type="file" name="file" id="file" onchange="selectFile(this);" accept=".png, .jpg, .jpeg, .gif" multiple>
    <input type="button" value="Submit" onclick="sendFile();">
    <div id="fileList"></div>
    <script>
        const selectFileArray = [];

        function selectFile(inputFile) {
            const files = Array.from(document.getElementById('file').files);
            let file;
            let otherCnt=0;

            while(file = files.shift()) {
                console.log(file);
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

                if(['.png', '.jpg', '.jpeg', '.gif'].includes(ext) === false) {
                    otherCnt += 1;
                    continue;
                }

                selectFileArray.push(file);

                let pTag = document.createElement('p');
                let text = document.createTextNode(name);
                pTag.appendChild(text);
                document.getElementById('fileList').appendChild(pTag);
                resetFile();
            }

            if(otherCnt > 0) {
                alert('이미지가 아닌 파일이 ' + otherCnt + '개 있습니다');
            }

            resetFile();
        }

        function sendFile() {
            if(selectFileArray.length === 0) {
                alert('업로드 가능한 이미지가 없습니다');
                return;
            }

            var formData = new FormData();
            var xmlHttp = new XMLHttpRequest();

            selectFileArray.forEach(file => { formData.append('file', file); });

            xmlHttp.onreadystatechange = function() {
                if(this.readyState == XMLHttpRequest.DONE) {
                    resetFile();
                    if(this.status == 200 ) {
                        let res = this.response;
                        if(res !== void 0 && res !== null && res.result) {

                            selectFileArray.length = 0;
                            let elList = document.getElementById('fileList');
                            while ( elList.hasChildNodes() ) {
                                elList.removeChild(elList.firstChild);
                            }
                            
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
        
        function resetFile() {
            document.getElementById('file').value = '';
        }
    </script>
</body>
```

다음 파트에서는 sharp 모듈을 사용한 이미지 리사이징을 다룹니다.

### REF

* [MDN Web Docs - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
* [MDN Web Docs - Using_FormData_Objects](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects)
* [TCP SCHOOL - xml_dom_xmlHttpRequest](https://www.tcpschool.com/xml/xml_dom_xmlHttpRequest)
* [MDN Web Docs - FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList)