# Express.js 이미지 업로드 파트 6

파트 6 에서는 드래그로 이미지 순서 변경하기를 작성합니다.

```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .draggable {
            width:150px;
            height:150px;
            object-fit:cover;
            box-sizing: border-box;
        }
        .draggable.active {
            border:5px solid #F00;
        }
    </style>
</head>
<body>
    <input type="file" name="file" id="file" onchange="selectFile(this);" accept=".png, .jpg, .jpeg, .gif" multiple>
    <input type="button" value="Submit" onclick="sendFile();">
    <div id="fileList"></div>
    <script>
        const selectFileArray = [];

        document.getElementById('fileList').addEventListener('dragover', (e)=>{
            e.preventDefault();
            let draggableElements = Array.from(document.getElementById('fileList').children);
            let drag = draggableElements.filter((el,idx) => el.getAttribute('data-dragging') == 'true')[0];

            let x = e.clientX, y = e.clientY;
            const element = draggableElements.reduce((accumulator, value) => {
                const box = value.getBoundingClientRect();
                const offsetX = x - box.left - box.width / 2;
                const offsetY = y - box.top - box.height;

                if(offsetX < 0 && accumulator.offsetX < offsetX && offsetY < 0 && accumulator.offsetY < offsetY) {
                    return { element : value, offsetX : offsetX, offsetY : offsetY};
                }else{
                    return accumulator;
                }

            }, { offsetX: Number.NEGATIVE_INFINITY, offsetY: Number.NEGATIVE_INFINITY }).element;

            if (element == null) {
                document.getElementById('fileList').appendChild(drag);
            } else {
                document.getElementById('fileList').insertBefore(drag, element);
            }
        });

        function selectFile(inputFile) {
            const files = Array.from(document.getElementById('file').files);
            let file;
            let otherCnt=0;

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
                }

                if(['.png', '.jpg', '.jpeg', '.gif'].includes(ext) === false) {
                    otherCnt += 1;
                    continue;
                }

                let idx = selectFileArray.push(file);

                let reader = new FileReader();
                reader.onload = function(event) {
                    let img = document.createElement('img');
                    img.setAttribute('src', event.target.result);
                    img.setAttribute('data-idx', idx);
                    img.classList.add('draggable');
                    
                    img.setAttribute('draggable', 'true');
                    img.addEventListener('dragstart', ()=>{
                        img.setAttribute('data-dragging', 'true');
                        img.classList.add('active');
                    });
                    img.addEventListener('dragend', ()=>{
                        img.removeAttribute('data-dragging');
                        img.classList.remove('active');
                    });

                    document.getElementById('fileList').appendChild(img);
                };
                reader.readAsDataURL(file);
                
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

            Array.from(document.getElementById('fileList').children).forEach(img => {
                formData.append('file', selectFileArray[(Number(img.getAttribute('data-idx'))-1)] );
            });

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
</html>
```

다음 파트에서는 드래그 앤 드롭으로 이미지 업로드를 다룹니다.

### REF
