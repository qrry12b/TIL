# HTML5_Canvas

# Canvas drawImage

MDN Web Docs Syntax
```
void ctx.drawImage(image, dx, dy);
void ctx.drawImage(image, dx, dy, dWidth, dHeight);
void ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

drawImage 에 이미지를 전달할때 주의할 점은 image 에 전달되는 시점의 정보로 그려지며  로드되기 전 이미지를 전달할 경우 캔버스에 그려지지 않습니다.   

또한 double buffering을 위해 다른 캔버스에 그린 다음 해당 캔버스를 그릴 경우에도 동일하게 그리는 도중(비동기)일 경우 최종이 아닌 Canvas가 전달되는 시점에서 그려집니다.   

drawImage(image, dx, dy)는 단순히 특정 좌표에 그릴때 호출할 수 있습니다.   

drawImage(image, dx, dy, dWidth, dHeight)는 이미지가 그려질 좌표와 이미지 크기를 지정할 필요가 있을때 호출할 수 있습니다. 예를들어 캔버스 크기에 맞춰 배경 이미지를 그릴 때 호출할 수 있습니다.   

drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)는  단일 이미지가 여러 이미지를 가지고 있는 경우(CSS Image Sprites) 이미지에서 특정 위치만 Canvas에 그릴 때 호출할 수 있습니다.   
특정한 위치가 필요한 경우가 아니라면 불필요한 연산을 할 수 있기 때문에 상황에 맞춰 호출하면 됩니다.   

또한 같은 이미지가 drawImage에서 매번 고정된 크기로 조정된다면 offscreen에 미리 그리고(캐시) offscreen을 그려 반복 연산을 줄일 수 있습니다.   

ex) 좌표 10.10에 있는 크기 10.10의 이미지를 5번 그리기
```  
offscreen.drawImage(img, 0,0, 10,10,10,10,10,10);
ctx.drawImage(offscreen, 0, 0);
ctx.drawImage(offscreen, 45, 45);
ctx.drawImage(offscreen, 0, 90);
ctx.drawImage(offscreen, 90, 0);
ctx.drawImage(offscreen, 90, 90);
```

단, 간단한 작업에서 불필요한 더블버퍼링은 성능 하락이 발생 할 수 있습니다.

### REF
* [MDN Web Docs #1](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)
* [MDN Web Docs #2](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage)

- - - - -

# Canvas drawRect

사각 테두리
```
ctx.fillRect(x, y, width, height)
```

사각 채우기
```
ctx.strokeRect(x, y, width, height)
```

사각 지우기
```
ctx.clearRect(x, y, width, height)
```

- - - - -

테두리가 둥근 사각형 그리기
```
const x=0, y=0, width = 10, height = 10;
const topLeft=3, topRight=3, bottomLeft=3, bottomRight=3;

ctx.beginPath();
ctx.moveTo(x + topLeft, y);
ctx.lineTo(x + width - topRight, y);
ctx.quadraticCurveTo(x + width, y, x + width, y + topRight);
ctx.lineTo(x + width, y + height - bottomRight);
ctx.quadraticCurveTo(x + width, y + height, x + width - bottomRight, y + height);
ctx.lineTo(x + bottomLeft, y + height);
ctx.quadraticCurveTo(x, y + height, x, y + height - bottomLeft);
ctx.lineTo(x, y + topLeft);
ctx.quadraticCurveTo(x, y, x + topLeft, y);
ctx.closePath();
```

### REF
* [MDN Web Docs](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes)
* [Stack Overflow-q-1255512](https://stackoverflow.com/questions/1255512/how-to-draw-a-rounded-rectangle-using-html-canvas)

- - - - -

# Canvas drawArc

원 그리기 (stroke 혹은 fill 호출 전까지는 그려지지 않습니다)
```
arc(x, y, radius, startAngle, endAngle, anticlockwise)
```

둥근 모서리 그리기 (stroke 혹은 fill 호출 전까지는 그려지지 않습니다)
```
arcTo(x1, y1, x2, y2, radius)
```

타원형 호 그리기 (stroke 혹은 fill 호출 전까지는 그려지지 않습니다)
```
ellipse(x, y, radiusX, radiusY, rotation, startAngle, endAngle [, counterclockwise]);
```

```
ctx.beginPath();
ctx.arc(100, 75, 50, 0, 2 * Math.PI);
ctx.stroke();
```

### REF
* [MDN Web Docs-arc](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/arc)
* [MDN Web Docs-ellipse](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/ellipse)
- - - - -

# Canvas ContextAttributes

컨텍스트 생성 시 컨텍스트 속성을 요청할 수 있습니다   
```
getContextAttributes();
```

ex)   
```
var ctx = canvas.getContext('2d', {alpha:false});
var isAlpha = ctx.getContextAttributes().alpha;
if(isAlpha) {
    ctx.globalAlpha = 0.7;
}
```

* [MDN Web Docs-getContextAttributes](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/getContextAttributes)
- - - - -

# Canvas drawText

주어진 (x, y) 위치에 주어진 텍스트를 채웁니다.
```
CanvasRenderingContext2D.fillText(text, x, y [, maxWidth])
```

주어진 (x, y) 위치에 주어진 텍스트를 칠(stroke)합니다.
```
CanvasRenderingContext2D.strokeText(text, x, y [, maxWidth])
```

텍스트 스타일 설정
```
CanvasRenderingContext2D.font = value
```

텍스트 정렬 설정 (default : start)
```
CanvasRenderingContext2D.textAlign = 'start' | 'end' | 'left' | 'right' | 'center'
```

베이스라인 정렬 설정 (default : alphabetic)
```
CanvasRenderingContext2D.textBaseline = 'top' | 'hanging' | 'middle' | 'alphabetic' | 'ideographic' | 'bottom'
```

글자 방향을 지정합니다 (default : inherit)
```
CanvasRenderingContext2D.direction = 'ltr' | 'rtl' | 'inherit'
```

웹 폰트 설정해서 텍스트 작성하기
```
let family = 'myFontFamily', text = 'Hello,Canvas', isExist = false;
document.fonts.forEach((v,i)=>{ if(v.family === family) isExist = true; });

if(isExist) {
    ctx.font = '60px '+family;
    ctx.strokeText(text, 10, 10);
    ctx.fillText(text, 10, 10);
}else{
    const ff = new FontFace(family, 'url(fontPath)');
    //document.fonts.add(ff);
    ff.load().then((font) => {
        document.fonts.add(font);
        ctx.font = '60px '+family;
        ctx.strokeText(text, 10, 10);
        ctx.fillText(text, 10, 10);
    }).catch((e)=>{ console.log('Error => ',e) });
}
```

키보드 입력에 따라 텍스트 작성하기 (키 입력이 발생할때마다 캔버스를 지우고 다시 그려야 합니다)
```

let input = document.createElement('input');
input.setAttribute('style':'position:absolute;display:block;height:0;overflow:hidden;');

input.addEventListener('keypress',(e)=>{
    canvas.width = canvas.width;
    ctx.fillText(e.target.value, 10, 10);
});
input.addEventListener('keyup',(e)=>{
    if(e.code.toLowerCase() === 'Escape'.toLowerCase() 
        || e.code.toLowerCase() === 'Enter'.toLowerCase()
        || e.keyCode === 27
        || e.keyCode === 13) {
        document.body.contains(e.target) && e.target.blur();
        return;
    });
    canvas.width = canvas.width;
    ctx.fillText(e.target.value, 10, 10);
});
input.addEventListener('blur',(e)=>{
    document.body.contains(e.target) && document.body.removeChild(e.target);
});
document.body.appendChild(input);
input.focus();
```

fonts에 family에 해당하는 FontFace가 있다면 그리기 작업을 진행하고   
없다면 새로운 FontFace를 생성하고 로드가 성공하면 텍스트 그리기 작업을 진행합니다.

포커스를 잃었을때 Element를 제거합니다. (단, focusout에서 Element 제거 코드를 작성하지 않도록 합니다, 제거되었을시 더이상 이벤트 전파가 발생하지 않도록 처리합니다)

### REF
* [MDN Web Docs - FontFace](https://developer.mozilla.org/en-US/docs/Web/API/FontFace)
* [MDN Web Docs - Drawing Text](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API/Tutorial/Drawing_text)