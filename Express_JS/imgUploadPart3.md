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

지난 파트에서 작성한 코드는 단일파일 기준이므로 기존 코드를 조금 수정합니다. 

우선 파일이 선택되었는지 확인하는 방법은 length 가 0인지, 그 다음에는 for문으로 파일 확장명이 png, jpg, jpeg, gif 인지 체크합니다.

여기서 Array.prototype.includes 는 배열이 특정 요소를 포함하고 있는지 판별합니다.

이미지 파일들만 선택되었다면 formData 에 append 하지만 files는 배열이 아닌 유사 배열이므로 forEach 메소드는 사용할 수 없기 때문에 Array.prototype.forEach.call 에 전달해서 호출합니다.

위와 같이 작성하면 선택된 파일중에 조건을 만족하지 못하는 파일이 하나라도 있을 경우 업로드를 하지 않게 됩니다.

아래에서 작성하는 코드는 조건을 통과한 파일만 전송하고 통과하지 못한 파일들은 카운트를 alter로 표시하도록 합니다.

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

수정된 코드에서는 imgArray, otherArray를 만들어 각각 조건을 통과한 파일과 통과하지 못한 파일들을 담고 있습니다.

이미지 파일에 대한 체크 후 imgArray를 다시 files 변수에 담을때 ...Array 는 Spread syntax 라고 하며 배열 또는 객체를 하나하나 넘기는 용도로 사용됩니다.

단순히 배열에 push(imgArray)를 하면 배열 그대로 push 되지만 ...Array 는 배열 안의 아이템이 하나씩 push 됩니다.

아래에서 작성하는 코드는 파일을 선택할때 조건을 통과한 파일만 목록에 출력하고 조건을 통과하지 못한 파일은 카운트를 alter를 통해 알려줍니다.

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

이 예시에서는 input:file 에 accept 속성을 추가했습니다.

accept 는 파일 선택시 허용할 파일 유형,파일 확장명을 나타내는 문자열로 각 타입 은 쉼표로 구분합니다.

파일 선택시 별도의 파일 타입 제한이 없다면 선언할 필요 없지만 이 예시와 같이 이미지 파일만 선택한다면 Javascript로 확인하기 전에 accept 속성을 통해 특정 파일만 선택하도록 할 수 있습니다.

단, accept 를 통해 허용할 파일 유형을 지정한다고 해도 파일 선택창에서 "모든 파일" 을 선택할 수 있기 때문에 Javascript 에서도 확인이 필요합니다.

조건(이미지 파일)을 통과한 파일은 div#fileList 를 찾아 안에 파일명을 append 하고 파일들은 배열에 추가합니다.

selectFile 함수에서 이미지 파일만 선택되도록 체크하며 
sendFile 함수에서는 단순히 배열이 비어있는지만 확인 후 파일을 전송합니다.

다음 파트에서는 sharp 모듈을 사용한 이미지 리사이징을 다룹니다.

### REF

* [MDN Web Docs - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
* [MDN Web Docs - Using_FormData_Objects](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects)
* [TCP SCHOOL - xml_dom_xmlHttpRequest](https://www.tcpschool.com/xml/xml_dom_xmlHttpRequest)
* [MDN Web Docs - FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList)
* [MDN Web Docs - Spread_syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
* [MDN Web Docs - Input/file](https://developer.mozilla.org/ko/docs/Web/HTML/Element/Input/file)