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