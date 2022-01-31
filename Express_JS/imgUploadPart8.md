# Express.js 이미지 업로드 파트 8

파트 8 에서는 간단한 이미지 편집 에디터를 작성합니다.

```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        canvas {width: 600px;height: 300px;}
    </style>
</head>
<body>
    <div>
        <input type="button" value="Load" id="load">
        <input type="button" value="Save" id="save">
        <input type="button" value="Rect" id="rect">
        <input type="button" value="Circle" id="circle">
        <input type="button" value="Text" id="Text">
        <input type="button" value="Path" id="Path">
    </div>
    <canvas id="canvas" width="600px" height="300px">Your browser doesn't support Canvas</canvas>
    <script>
        var canvas, items = [];
        function cRect(e) {
            let rect = canvas.getBoundingClientRect();
            return {width: rect.width, height: rect.height, x:e.clientX-rect.x, y:e.clientY-rect.y};
        }
        document.addEventListener("DOMContentLoaded", () => {
            canvas = document.getElementById('canvas');
            canvas.getContext('2d').fillStyle = '#FFFFFF';
            canvas.getContext('2d').fillRect(0, 0, canvas.width, canvas.height);

            canvas.addEventListener('click',(e)=>{});
            canvas.addEventListener('mouseout',(e)=>{});
            canvas.addEventListener('mouseup',(e)=>{});
            canvas.addEventListener('mousedown',(e)=>{});
            canvas.addEventListener('mousemove',(e)=>{});
            canvas.addEventListener('touchstart',(e)=>{});
            canvas.addEventListener('touchend',(e)=>{});
            canvas.addEventListener('touchmove',(e)=>{});

            document.getElementById('save').addEventListener('click',(e)=>{
                canvas.toBlob((blob)=>{
                    let url  = URL.createObjectURL(blob);

                    let d = new Date();
                    let fileName = d.getFullYear() +'_'+ (d.getMonth()+1) +'_'+ d.getDate() +'_'+ d.getHours()+'_'+d.getMinutes()+'_'+d.getSeconds()+'_'+(Math.floor(100 + Math.random()*100));

                    let downloadLink = document.createElement('a');
                    downloadLink.href = url;
                    downloadLink.download = fileName+'.png';
                    downloadLink.click();
                }, 'image/png');             
            });

            document.getElementById('load').addEventListener('click',(e)=>{
                let input = document.createElement('input');
                input.addEventListener('change',(e)=>{
                    canvas.getContext('2d').fillStyle = '#FFFFFF';
                    canvas.getContext('2d').fillRect(0, 0, canvas.width, canvas.height);

                    if(e.target.files !== void 0 && e.target.files !== null && e.target.files.length > 0) {
                        items = [];
                        let file = e.target.files[0];
                        let reader = new FileReader();
                        reader.onload = function(event) {
                            let img = document.createElement('img');
                            img.onload = () => {
                                let w = img.width;
                                let h = img.height;
                                if(w > canvas.width) {
                                    let r = h / w;
                                    w = canvas.width;
                                    h = Math.floor(w * r);
                                }
                                if(h > canvas.height) {
                                    let r = w / h;
                                    h = canvas.height;
                                    w = Math.floor(h * r);
                                }
                                canvas.getContext('2d').drawImage(img, 0, 0, w, h);
                                items.push({
                                    type : 'img',
                                    width : w,
                                    height : h,
                                    x : 0,
                                    y : 0,
                                    img : img
                                });
                            }
                            img.setAttribute('src', event.target.result);
                        };
                        reader.readAsDataURL(file);
                    }
                });
                input.setAttribute('type','file');
                input.click();
            });
        });
    </script>
</body>
</html>

<!-- TODO -->
```

### REF
* [MDN Web Docs - HTML_Drag_and_Drop_API](https://developer.mozilla.org/ko/docs/Web/API/HTML_Drag_and_Drop_API)
* [MDN Web Docs - DataTransfer](https://developer.mozilla.org/ko/docs/Web/API/DataTransfer)
* [MDN Web Docs - FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList)
* [MDN Web Docs - File](https://developer.mozilla.org/en-US/docs/Web/API/File)
* [MDN Web Docs - FileReader](https://developer.mozilla.org/ko/docs/Web/API/FileReader)
* [MDN Web Docs - Canvas_API](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API)
* [MDN Web Docs - HTMLCanvasElement toDataURL](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toDataURL)
* [MDN Web Docs - atob](https://developer.mozilla.org/en-US/docs/Web/API/atob)
* [MDN Web Docs - Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)
* [MDN Web Docs - Blob](https://developer.mozilla.org/ko/docs/Web/API/Blob)
* [MDN Web Docs - a#attr-download](https://developer.mozilla.org/ko/docs/Web/HTML/Element/a#attr-download)
* [MDN Web Docs - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
* [MDN Web Docs - Using_FormData_Objects](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects)
* [MDN Web Docs - getBoundingClientRect](https://developer.mozilla.org/ko/docs/Web/API/Element/getBoundingClientRect)
* [TCP SCHOOL - xml_dom_xmlHttpRequest](https://www.tcpschool.com/xml/xml_dom_xmlHttpRequest)