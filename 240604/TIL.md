# TIL (Today I Learned)

## 2024-06-04

### 주제: 헷갈렸던 개념들

#### 자바스크립트에서 얕은복사와 깊은복사에 개념 그리고 깊은 복사를 하려면 어떻게하는지?

얕은 복사 (Shallow Copy)

- 얕은 복사는 객체의 최상위 레벨의 속성들만 복사하고, 중첩된 객체나 배열은 원본 객체와 참조를 공유합니다. 즉, 중첩된 객체의 변경이 복사된 객체에도 영향을 미치게 됩니다.

예시: 

````
let original = { a: 1, b: { c: 2 } };
let shallowCopy = Object.assign({}, original);

console.log(shallowCopy); // { a: 1, b: { c: 2 } }

shallowCopy.b.c = 3;
console.log(original.b.c); // 3 (원본 객체의 중첩된 객체도 변경됨)

````

또 다른 예시로, 스프레드 연산자를 사용할 수도 있습니다:

````

let shallowCopy2 = { ...original };

````

깊은 복사 (Deep Copy)

- 깊은 복사는 객체의 모든 레벨의 속성들까지 모두 복사합니다. 중첩된 객체나 배열도 재귀적으로 복사되며, 원본 객체와 독립적인 새로운 객체를 만듭니다.

JSON.parse()와 JSON.stringify() 사용: 이 방법은 객체가 JSON으로 직렬화 가능한 경우에만 사용 가능합니다. 즉, 함수나 undefined 같은 값이 포함된 객체는 제대로 복사되지 않습니다.

````
let original = { a: 1, b: { c: 2 } };
let deepCopy = JSON.parse(JSON.stringify(original));

console.log(deepCopy); // { a: 1, b: { c: 2 } }

deepCopy.b.c = 3;
console.log(original.b.c); // 2 (원본 객체는 변경되지 않음)

````

재귀 함수를 이용한 깊은 복사: 직접 재귀 함수를 작성하여 깊은 복사를 수행할 수 있습니다.

````
function deepClone(obj) {
    if (obj === null || typeof obj !== 'object') {
        return obj;
    }

    if (Array.isArray(obj)) {
        let copy = [];
        obj.forEach((elem, index) => {
            copy[index] = deepClone(elem);
        });
        return copy;
    }

    let copy = {};
    Object.keys(obj).forEach((key) => {
        copy[key] = deepClone(obj[key]);
    });
    return copy;
}

let original = { a: 1, b: { c: 2 } };
let deepCopy = deepClone(original);

console.log(deepCopy); // { a: 1, b: { c: 2 } }

deepCopy.b.c = 3;
console.log(original.b.c); // 2 (원본 객체는 변경되지 않음)

````

라이브러리 사용: Lodash 같은 라이브러리를 사용하여 깊은 복사를 수행할 수 있습니다.

````
// Lodash 사용 예시
let _ = require('lodash');

let original = { a: 1, b: { c: 2 } };
let deepCopy = _.cloneDeep(original);

console.log(deepCopy); // { a: 1, b: { c: 2 } }

deepCopy.b.c = 3;
console.log(original.b.c); // 2 (원본 객체는 변경되지 않음)

````

#### 리액트의 useState에서 왜 상태값을 바꿀 때 setState 를 사용하는지?

- 상태 변경 감지: React는 상태(state)나 속성(props)의 변경을 감지하여 컴포넌트를 다시 렌더링합니다. useState 훅을 사용하면 React는 상태가 변경될 때 컴포넌트를 다시 렌더링할 수 있도록 내부적으로 상태 변경을 추적합니다. setState 함수는 React에게 상태가 변경되었음을 알리는 역할을 합니다.

- 불벙성 유지: React의 상태 관리 철학은 불변성을 유지하는 것입니다. 불변성을 유지함으로써 이전 상태와 새로운 상태를 쉽게 비교할 수 있습니다. setState를 사용하면 상태를 직접 변경하는 대신 새로운 상태를 제공하여 불변성을 유지할 수 있습니다.

- 렌더링 최적화: setState 함수는 상태 변경이 발생할 때마다 컴포넌트를 다시 렌더링하지만, React는 내부적으로 여러 상태 변경을 하나로 병합하고 최적화하여 렌더링 성능을 높입니다. 직접 상태를 변경하면 이러한 최적화가 불가능합니다.

- 클로저 문제 해결: useState와 함께 제공되는 setState 함수는 React의 상태 변경 시스템과 연계되어 클로저 문제를 해결합니다. 상태 변경 함수는 항상 최신 상태 값을 참조하여 상태 업데이트가 일관되게 이루어지도록 합니다.