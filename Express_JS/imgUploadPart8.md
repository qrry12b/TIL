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
        <input type="button" value="Upload" id="upload">
        <input type="button" value="Rect" id="rect">
        <input type="button" value="Circle" id="circle">
        <input type="button" value="Line" id="line">
    </div>
    <canvas id="canvas" width="600px" height="300px">Your browser doesn't support Canvas</canvas>
    <script>
        function $Canvas(id) {
            if(this instanceof $Canvas === false) {
                return new $Canvas(id);
            }

            let canvas = this.canvas = document.getElementById(id);
            if(canvas === void 0 || canvas === null) throw new Error('$Canvas getElementById');
            
            let ctx = this.ctx = canvas.getContext('2d');
            if(ctx === void 0 || ctx === null) return;

            $Canvas.prototype.whiteClear.call(this);
        }

        $Canvas.prototype.toBlob = function(callback, type, quality) {
            if(callback === void 0 || callback === null || typeof callback !== 'function') return;
            this.canvas.toBlob(callback, type, quality);
        }

        $Canvas.prototype.whiteClear = function() {
            this.clear('#FFFFFF');
        }

        $Canvas.prototype.clear = function(color) {
            let canvas = this.canvas, ctx = this.ctx;
            if(typeof color === 'string') {
                ctx.fillStyle = color;
                ctx.fillRect(0, 0, canvas.width, canvas.height);
            }
            else if(ctx.getContextAttributes().alpha == false) {
                ctx.fillStyle = '#FFFFFF';
                ctx.fillRect(0, 0, canvas.width, canvas.height);
            }else{
                ctx.clearRect(0, 0, canvas.width, canvas.height);
            }
        }

        $Canvas.prototype.download = function(fileName) {
            this.toBlob((blob)=>{
                let url  = URL.createObjectURL(blob);

                let downloadLink = document.createElement('a');
                downloadLink.href = url;
                downloadLink.download = fileName;
                downloadLink.click();
            }, 'image/png', null);
        }

        $Canvas.prototype.drawImage = function(...param) {
            this.ctx.drawImage(...param);
        }

        $Canvas.prototype.lineCap = function(lineCap) {
            let table = ['butt', 'round', 'square'];
            this.ctx.lineCap = lineCap;
        }

        $Canvas.prototype.lineWidth = function(width) {
            if(width !== void 0 && width !== null)
            this.ctx.lineWidth = width;;   
        }

        $Canvas.prototype.strokeStyle = function(color) {
            if(color !== void 0 && color !== null)
            this.ctx.strokeStyle = color;
        }

        $Canvas.prototype.drawLine = function(x1, y1, x2, y2, color, width) {
            this.strokeStyle(color);
            this.lineWidth(width);

            this.ctx.beginPath();
            this.ctx.moveTo(x1, y1);
            this.ctx.lineTo(x2, y2);
            this.ctx.stroke();
            this.ctx.closePath();
        }


        $Canvas.prototype.drawRect = function(x1, y1, x2, y2, color, width) {
            this.strokeStyle(color);
            this.lineWidth(width);

            if(x1 > x2) {
                let temp = x1;
                x1 = x2;
                x2 = temp;
            }

            if(y1 > y2) {
                let temp = y1;
                y1 = y2;
                y2 = temp;
            }

            this.ctx.strokeRect(x1, y1, x2 - x1, y2 - y1);
        }

        $Canvas.prototype.drawArc = function(x1, y1, x2, y2, color, width) {
            this.strokeStyle(color);
            this.lineWidth(width);

            let w = Math.abs(x2 - x1);
            let h = Math.abs(y2 - y1);

            let p1 = x2 - x1 < 0 ? -1 : 1;
            let p2 = y2 - y1 < 0 ? -1 : 1;

            let arc = Math.min(w,h);

            this.ctx.beginPath();
            this.ctx.arc(x1 + (arc/2) * p1, y1 + (arc/2) * p2, arc/2, 0, Math.PI * 2, true);
            this.ctx.stroke();
        }

        $Canvas.prototype.width = function(param) {
            if(param === void 0) return this.canvas.width;
            this.canvas.width = param;
            return this;
        }

        $Canvas.prototype.height = function(param) {
            if(param === void 0) return this.canvas.height;
            this.canvas.height = param;
            return this;
        }

        $Canvas.prototype.event = function(evName, callback) {
            if(callback === void 0 || callback === null || typeof callback !== 'function') return;

            if(Array.isArray(evName)) {
                for(let i=0; i<evName.length; i++) {
                    this.event(evName[i], callback);
                }
                return this;
            }

            let o = {callback : callback, canvas: this.canvas};
            this.canvas.addEventListener(evName, function(e) {
                let rect = o.canvas.getBoundingClientRect();
                o.callback({width: rect.width, height: rect.height, x:e.clientX-rect.x, y:e.clientY-rect.y});
            });

            return this;
        }

        $Canvas.prototype.cursor = function(style) {
            let table = [/*draw*/'crosshair', /*move*/'move', /*click*/'pointer', /*default*/'default'];
            this.canvas.style.cursor = style;
            return this;
        }

        let canvas = null;

        let load = null; 
        let save = null;
        let upload = null;
        let rect = null;
        let circle = null;
        let line = null;

        let items = [];
        let flagParam = ['none'];
    
        
        function update() {
            canvas.whiteClear();

            for(let i=0; i<items.length; i++) {
                let item = items[i];
                let type = item[0];
                let param = [...item].splice(1, item.length);

                if(type === 'img') {
                    canvas.drawImage( ...param );
                }else if(type === 'line') {
                    canvas.drawLine( ...param );
                }else if(type === 'rect') {
                    canvas.drawRect( ...param );
                }else if(type === 'circle') {
                    canvas.drawArc( ...param );
                }
            }

            if(['line', 'rect', 'circle'].includes(flagParam[0])) {
                let x1 = (flagParam[1] || {}).x || 0;
                let y1 = (flagParam[1] || {}).y || 0;
                let x2 = (flagParam[2] || {}).x || x1;
                let y2 = (flagParam[2] || {}).y || y1;

                if(flagParam[0] === 'line'){
                    canvas.drawLine(x1,y1, x2, y2, '#000000', 1);
                }
                else if(flagParam[0] === 'rect'){
                    canvas.drawRect(x1, y1, x2, y2, '#000000', 1);
                }
                else if(flagParam[0] === 'circle'){
                    canvas.drawArc(x1, y1, x2, y2, '#000000', 1);
                }
            }
        }

        function gId(id) { return document.getElementById(id); }
        
        document.addEventListener("DOMContentLoaded", () => {
            canvas = $Canvas('canvas')
            .event('click',(e)=>{})
            .event(['mouseup', 'mouseout'],(e)=>{
                canvas.cursor('default');
                if(['line','rect','circle'].includes(flagParam[0])) {
                    flagParam[2] = e;
                    items.push([flagParam[0], (flagParam[1] || {}).x || 0, (flagParam[1] || {}).y || 0, (flagParam[2] || {}).x || 0, (flagParam[2] || {}).y || 0,'#000000', 1]);
                    flagParam = ['none'];
                    update();
                }
            })
            .event('mousemove',(e)=>{
                if(['line','rect','circle'].includes(flagParam[0]) && flagParam.length > 1) {
                    flagParam[2] = e;
                    update();
                }
            })
            .event('mousedown',(e)=>{
                if(['line','rect','circle'].includes(flagParam[0])) {
                    flagParam[1] = e;
                    update();
                }
            })
            .event('touchstart',(e)=>{})
            .event('touchend',(e)=>{})
            .event('touchmove',(e)=>{});

            load = gId('load');
            save = gId('save');
            upload = gId('upload');
            rect = gId('rect');
            circle = gId('circle');
            line = gId('line');

            line.addEventListener('click',(e)=>{
                flagParam = ['line'];
                canvas.cursor('crosshair');
            });

            rect.addEventListener('click',(e)=>{
                flagParam = ['rect'];
                canvas.cursor('crosshair');
            });

            circle.addEventListener('click',(e)=>{
                flagParam = ['circle'];
                canvas.cursor('crosshair');
            });

            upload.addEventListener('click',(e)=>{
                flagParam = ['none'];
                canvas.cursor('default');

                canvas.toBlob((blob)=>{
                    let url  = URL.createObjectURL(blob);
                    let formData = new FormData();
                    var xmlHttp = new XMLHttpRequest();

                    let d = new Date();
                    let fileName = d.getFullYear() +'_'+ (d.getMonth()+1) +'_'+ d.getDate() +'_'+ d.getHours()+'_'+d.getMinutes()+'_'+d.getSeconds()+'_'+(Math.floor(100 + Math.random()*100)) + '.png';

                    formData.append('file', blob, fileName);
 
                    xmlHttp.onreadystatechange = function() {
                        if(this.readyState == XMLHttpRequest.DONE) {
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
            }, 'image/png', null);
            });

            save.addEventListener('click',(e)=>{
                flagParam = ['none'];
                canvas.cursor('default');

                let d = new Date();
                let fileName = d.getFullYear() +'_'+ (d.getMonth()+1) +'_'+ d.getDate() +'_'+ d.getHours()+'_'+d.getMinutes()+'_'+d.getSeconds()+'_'+(Math.floor(100 + Math.random()*100)) + '.png';
                canvas.download(fileName);
            });

            load.addEventListener('click',(e)=>{
                flagParam = ['none'];
                canvas.cursor('default');

                let input = document.createElement('input');
                input.addEventListener('change',(e)=>{
                    canvas.whiteClear();
                    if(e.target.files !== void 0 && e.target.files !== null && e.target.files.length > 0) {
                        items = [];
                        let file = e.target.files[0];
                        let reader = new FileReader();
                        reader.onload = function(event) {
                            let img = document.createElement('img');
                            img.onload = () => {
                                let w = img.width;
                                let h = img.height;
                                if(w > canvas.width()) {
                                    let r = h / w;
                                    w = canvas.width();
                                    h = Math.floor(w * r);
                                }
                                if(h > canvas.height()) {
                                    let r = w / h;
                                    h = canvas.height();
                                    w = Math.floor(h * r);
                                }
                                canvas.drawImage(img, 0, 0, w, h);
                                items.push(['img', img, 0, 0, w, h]);
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
```

### REF
* [MDN Web Docs - Blob](https://developer.mozilla.org/ko/docs/Web/API/Blob)
* [MDN Web Docs - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
* [MDN Web Docs - a#attr-download](https://developer.mozilla.org/ko/docs/Web/HTML/Element/a#attr-download)
* [MDN Web Docs - Using_FormData_Objects](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects)
* [MDN Web Docs - getBoundingClientRect](https://developer.mozilla.org/ko/docs/Web/API/Element/getBoundingClientRect)
* [TCP SCHOOL - xml_dom_xmlHttpRequest](https://www.tcpschool.com/xml/xml_dom_xmlHttpRequest)
* [MDN Web Docs - Canvas_API](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API)
* [MDN Web Docs - HTMLCanvasElement toDataURL](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toDataURL)
* [MDN Web Docs - FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList)
* [MDN Web Docs - File](https://developer.mozilla.org/en-US/docs/Web/API/File)