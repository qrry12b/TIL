# Express.js 이미지 업로드 파트 7

파트 7 에서는 드래그 앤 드롭으로 이미지 업로드를 작성합니다.

```
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
    .dropzone {
        width: 150px;
        height: 150px;
        background-color: #999;
    }
</style>
```

```
<body>
    <input type="file" name="file" id="file" accept=".png, .jpg, .jpeg, .gif" multiple>
    <input type="button" value="Submit" id="sendButton">
    <div id="dropZone" class="dropZone"></div>
    <div id="fileList"></div>
    <script>
        const extTable = ['.png', '.jpg', '.jpeg', '.gif'];
        const selectFileArray = [];
        var dropZone, sendButton, inputFile, fileList;
        document.addEventListener("DOMContentLoaded", () => {
            dropZone = document.getElementById('dropZone');
            sendButton = document.getElementById('sendButton');
            inputFile = document.getElementById('file');
            fileList = document.getElementById('fileList');

            if(dropZone) {
                dropZone.addEventListener('drop',(e)=>{
                    e.preventDefault();
                    dropZone.style.backgroundColor = '#999';
                    if(e.dataTransfer.types.includes('Files')) {
                        getFiles(Array.from(e.dataTransfer.files));
                    }
                });
                dropZone.addEventListener('dragenter',(e)=>{
                    e.preventDefault();
                    if(e.dataTransfer.types.includes('Files')) {
                        dropZone.style.backgroundColor = '#F88';
                    }
                });
                dropZone.addEventListener('dragover',(e)=>{
                    e.preventDefault();
                    e.dataTransfer.dropEffect = "move";
                });
                dropZone.addEventListener('dragleave',(e)=>{
                    dropZone.style.backgroundColor = '#999';
                });
            }

            if(inputFile) {
                inputFile.addEventListener('change',(e)=>{
                    const files = Array.from(document.getElementById('file').files);
                    getFiles(files);
                });
            }

            if(fileList) {
                fileList.addEventListener('dragover', (e)=>{
                    e.preventDefault();
                    if(e.dataTransfer.types.includes('text/html')) {
                        let draggableElements = Array.from(fileList.children);
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
                            fileList.appendChild(drag);
                        } else {
                            fileList.insertBefore(drag, element);
                        }   
                    }
                });
            }

            if(sendButton) {
                sendButton.addEventListener('click',(e)=>{
                    sendFile();
                });
            }
        });

        function getFiles( files /*FileList*/ ) {
            let file;
            let otherCnt = 0;
            if(files && files.length) {
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

                    if(extTable.includes(ext) === false) {
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

                        if(fileList) fileList.appendChild(img);
                    };
                    reader.readAsDataURL(file);
                    resetFile();
                }
            }

            if(otherCnt > 0) {
                alert('이미지가 아닌 파일이 ' + otherCnt + '개 있습니다');
            }
        }

        function sendFile() {
            if(selectFileArray.length === 0) {
                alert('업로드 가능한 이미지가 없습니다');
                return;
            }

            var formData = new FormData();
            var xmlHttp = new XMLHttpRequest();

            if(fileList) {
                Array.from(fileList.children).forEach(img => {
                    formData.append('file', selectFileArray[(Number(img.getAttribute('data-idx'))-1)] );
                });
            }

            xmlHttp.onreadystatechange = function() {
                if(this.readyState == XMLHttpRequest.DONE) {
                    resetFile();
                    if(this.status == 200 ) {
                        let res = this.response;
                        if(res !== void 0 && res !== null && res.result) {

                            selectFileArray.length = 0;
                            let elList = fileList;
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

다음 파트에서는 간단한 이미지 편집 에디터를 다룹니다.

### REF
* [MDN Web Docs - HTML_Drag_and_Drop_API](https://developer.mozilla.org/ko/docs/Web/API/HTML_Drag_and_Drop_API)
* [MDN Web Docs - DataTransfer](https://developer.mozilla.org/ko/docs/Web/API/DataTransfer)
* [MDN Web Docs - FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList)
* [MDN Web Docs - File](https://developer.mozilla.org/en-US/docs/Web/API/File)
* [MDN Web Docs - FileReader](https://developer.mozilla.org/ko/docs/Web/API/FileReader)
* [MDN Web Docs - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
* [MDN Web Docs - Using_FormData_Objects](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects)
* [MDN Web Docs - getBoundingClientRect](https://developer.mozilla.org/ko/docs/Web/API/Element/getBoundingClientRect)
* [TCP SCHOOL - xml_dom_xmlHttpRequest](https://www.tcpschool.com/xml/xml_dom_xmlHttpRequest)