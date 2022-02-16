# HTMLElement 자식 노드 모두 지우기

```
function removeChildAll(node : Node) :void {
    while (node.lastChild) node.removeChild(node.lastChild);
}
```

firstChild 부터 지우는 예시도 있고 lastChild 부터 지우는 예시도 있지만   
속도 면에서 lastChild가 조금 더 빠르다고 합니다.

- - - - -

```
textContent = ''
```

textContent = '' 를 사용할 경우 innerHTML 과 달리 HTML 파서를 호출하지 않고 단일 text 노드로 대체되기 때문에 더 빠르다고 합니다.

단, MDN Browser compatibility을 참조하면 removeChild(node.lastChild) IE 6 에서 지원하지만 textContent는 IE 9부터 지원됩니다.

- - - - -

### REF
* [Stackoverflow](https://stackoverflow.com/a/3955238)