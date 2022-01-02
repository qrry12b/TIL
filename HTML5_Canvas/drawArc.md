# Canvas drawArc

원 그리기 (stroke 혹은 fill 호출 전까지는 그려지지 않습니다)
```
arc(x, y, radius, startAngle, endAngle, anticlockwise)
```

둥근 모서리 그리기 (stroke 혹은 fill 호출 전까지는 그려지지 않습니다)

```
arcTo(x1, y1, x2, y2, radius)
```

```
ctx.beginPath();
ctx.arc(100, 75, 50, 0, 2 * Math.PI);
ctx.stroke();
```

### REF
* [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/arc)