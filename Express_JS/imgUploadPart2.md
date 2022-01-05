# Express.js 이미지 업로드 파트 2

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
    <input type="file" name="file" id="file">
    <input type="button" value="Submit" onclick="sendFile();">
    <script>

        function sendFile() {
            let file = document.getElementById('file').files[0];

            if(file === void 0) {
                alert('파일을 선택해 주세요');
                return;
            }

            let name = file.name;

            if(name === void 0 || name === null || name === '') {
                alert('잘못된 파일 이름입니다');
                return;
            }

            let extIdx = name.lastIndexOf('.');
            let ext = '';
            if(0 <= extIdx) {
                ext = name.substring(extIdx);
            }

            if(ext !== '.png' && ext !== '.jpg' && ext !== '.jpeg' && ext !== '.git') {
                alert('이미지 파일을 선택해주세요 (png, jpg, jpeg, git)');
                return;
            }

            var formData = new FormData();
            var xmlHttp = new XMLHttpRequest();

            xmlHttp.onreadystatechange = function() {
                if(this.readyState == this.DONE) {
                    resetFile();
                    if(this.status == 200 ) {
                        let res = JSON.parse(xmlHttp.responseText);
                        if(res.result) {
                            alert('파일 업로드에 성공했습니다');
                            return;
                        }    
                    }
                    alert('파일 업로드에 실패했습니다 code:'+this.status);
                }
            };

            formData.append('file', file);

            xmlHttp.open("POST", "/upload");
            xmlHttp.send(formData);
        }
        
        function resetFile() {
            document.getElementById('file').value = '';
        }
    </script>
</body>
</html>
```

/#TODO#/

### REF

* [MDN Web Docs - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

* [MDN Web Docs - Using_FormData_Objects](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects)

* [TCP SCHOOL - xml_dom_xmlHttpRequest](https://www.tcpschool.com/xml/xml_dom_xmlHttpRequest)