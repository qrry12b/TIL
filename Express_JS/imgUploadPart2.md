# Express.js 이미지 업로드 파트 2

파트 2 에서는 파일 업로드 페이지를 작성합니다.

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

            formData.append('file', file);

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
        
        function resetFile() {
            document.getElementById('file').value = '';
        }
    </script>
</body>
</html>
```

IE 또는 구브라우저에서는 동작하지 않을 수 있습니다.

간단한 업로드를 구현하기 위해 input:file 태그와 input:button 만 추가했습니다.

버튼을 클릭할 경우 sendFile 함수를 호출하며 선택된 파일에 대해 유효성 검사를 합니다.

선택한 파일이 없다면 파일을 선택하도록 알림을 표시하고 종료합니다.

그 다음에는  파일 이름을 알 수 없거나 파일이름을 통해 이미지 파일이 가지는 확장명이 아닐경우 알림을 표시하고 종료합니다.

파일 타입 검사를 통과했다면 선택된 파일을 비동기로 전송하기 위해 FormData와 XMLHttpRequest 객체를 생성합니다.

FormData는 XMLHttpRequest의 send() 에 넘기기 위해 생성하였으며 append(이름, 값) 으로 전송할 데이터를 추가할 수 있습니다.

XMLHttpRequest 의 프로퍼티 onreadystatechange는 상태가 변경될때마다 호출되는 함수를 작성합니다.

함수 내부에서 요청이 완료되고 (readyState === XMLHttpRequest.DONE) 응답헤더가 성공 (state === 200) 일 경우 response 확인하도록 작성합니다.

지난번에 작성한 서버를 실행하고 파일을 전송해보면 콘솔 req.files 에 파일 정보를 확인 할 수 있습니다.

다음 파트에서는 다중 파일 업로드를 다룹니다.

### REF

* [MDN Web Docs - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
* [MDN Web Docs - Using_FormData_Objects](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects)
* [TCP SCHOOL - xml_dom_xmlHttpRequest](https://www.tcpschool.com/xml/xml_dom_xmlHttpRequest)