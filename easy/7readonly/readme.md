# readonly

읽기전용. 이를 통해 함수형 방식이 가능해진다. (예상되지 않는 상태 변경 허용 x)

```ts
type Foo = {
  readonly bar: number;
  readonly bas: number;
};

// 초기화는 오케이
let foo: Foo = { bar: 123, bas: 456 };

// 변경은 안됨
foo.bar = 456; // 오류: 상수나 읽기 전용 속성은 대입(assignment) 표현식의 좌항이 될 수 없음
```

## const 와의 차이점

const는 변수 참조를 위한 것이고 변수에 다른 값을 할당/대입할 수 없음.

하지만 readonly는 속성을 위한 것이고 속성을 엘리어싱을 통해 변경가능.

```ts
// 1
const foo = 123; // 변수 참조
var bar: {
  readonly bar: number; // 속성의 경우
};

// 2
let foo: {
  readonly bar: number;
} = {
  bar: 123,
};

function iMutateFoo(foo: { bar: number }) {
  foo.bar = 456;
}

iMutateFoo(foo); // foo 인자가 foo 파라미터에 의해 앨리어싱됨
console.log(foo.bar); // 456!
```

기본적으로 readonly는 내가 속성을 변경하지 못함을 보장하지만, 객체를 다른 사람에게 넘길 경우에는 이것이 보장되지 않고 그 다른 사람은 객체의 속성을 변경할 수 있습니다 (타입 호환성 문제 때문에 허용됨). 물론 iMutateFoo를 foo.bar를 변경하지 않는 함수로 선언했다면 아래에 보이는 것처럼 컴파일러가 올바르게 잘못을 지적할 수 있습니다:
