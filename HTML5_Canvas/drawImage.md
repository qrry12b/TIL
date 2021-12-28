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

ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)는  단일 이미지가 여러 이미지를 가지고 있을때(CSS Image Sprites) 이미지에서 특정 위치만 Canvas에 그릴 때 호출할 수 있습니다.   
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

단, 간단한 그리기 작업에서 불필요한 더블버퍼링은 성능 하락이 발생 할 수 있습니다.

REF
* https://developer.mozilla.org/ko/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas
* https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage