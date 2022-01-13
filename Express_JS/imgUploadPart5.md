# Express.js 이미지 업로드 파트 5

파트 5 에서는 업로드할 이미지 미리보기를 작성합니다.

```
// let pTag = document.createElement('p');
// let text = document.createTextNode(name);
// pTag.appendChild(text);
// document.getElementById('fileList').appendChild(pTag);

let reader = new FileReader();
reader.onload = function(event) {
    let img = document.createElement('img');
    img.setAttribute('src', event.target.result);
    img.setAttribute('style','width:150px; height:150px; object-fit:cover;');
    document.getElementById('fileList').appendChild(img);
};
reader.readAsDataURL(file);
```

### REF
* [MDN Web Docs - FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList)
* [MDN Web Docs - File](https://developer.mozilla.org/en-US/docs/Web/API/File)
* [MDN Web Docs - FileReader](https://developer.mozilla.org/ko/docs/Web/API/FileReader)