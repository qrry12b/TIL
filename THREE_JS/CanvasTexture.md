# CanvasTexture

CanvasTexture는 Texture를 상속받아 구현된 클래스로 생성자가 호출되는 시점에서 needsUpdate 를 true 로 가진다는 점 이외에는 Texture 클래스와 거의 동일합니다.   

또한 CanvasTexture 가 아닌 Texture 을 사용해도 needsUpdate 만 true로 설정하면 동일한 결과를 얻을 수 있습니다.

캔버스에 그려지는 데이터가 지속적으로 수정된다면 texture.map.needsUpdate 도 **갱신시 마다** true로 변경해야 합니다.

```
const canvas = document.createElement('canvas');
const ctx = canvas.getContext('2d', { alpha: false });

canvas.width = 128;
canvas.height = 64;

const canvasTexture = new THREE.CanvasTexture(canvas);
const materialOps = {
    map : canvasTexture,
    opacity : 0.6,
    transparent : true
}
const canvasMaterial = new THREE.MeshBasicMaterial( materialOps );
```

### REF
* [docs](https://threejs.org/docs/#api/en/textures/CanvasTexture)