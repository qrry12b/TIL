### Typescript Overloading

```
export function append(el : HTMLElement, node : Node):Node
export function append(el : HTMLElement, arr : Array<Node>):Array<Node>
export function append(el : HTMLElement, arg1 ?: any):Node|Array<Node> {
    if(QType.isArray(arg1)) {
        var arr = [];
        arg1.forEach((val,_idx,_arr)=>{
            arr.push(el.appendChild(val));
        })
    }else if(QType.isObject(arg1)) {
        return el.appendChild(arg1)
    }
}
```

타입스크립트를 작성하다 보면 하나의 함수에 매개변수 타입이나 갯수가 다를 수 있습니다.   

매개변수 타입이 다를경우 조합유형(Union Types)으로 인자마다 여러 타입을 지정해주는 방법도 있지만   
하나의 함수에 이런 Union Types의 매개변수가 많을 경우 타입스크립트의 지원을 충분히 받을 수 없는 경우도 있습니다.   

typescript 에서는 실제로 구현할 함수 위에 입력 가능한 인자들의 타입과 갯수를 작성하고   
가장 밑에서 실제 구현되는 함수에서 타입은 any 혹은 Union Types를 사용하고   
동적(옵션)인자는 ?: 을 선언해 생략할 수 있도록 하거나 = value를 작성해 기본값을 줄 수 있습니다.   

VSCode의 경우 호출된 함수 위에 마우스를 올리면 전달된 매개변수와 일치하는 함수 인자 갯수와 타입을 표시해줍니다.   

### REF
* [Typescript Handbook #overloads](https://www.typescriptlang.org/docs/handbook/functions.html#overloads)
* [Typescript Handbook #union-types](https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html#union-types)