#### JSON이란?
+ 값이나 객체를 나태내주는 범용 포맷이다.
+ JSON은 본래 자바스크립트에서 사용할 목적으로 만들어진 포맷이다.
+ 그런데 라이브러리를 사용하면 자바스크립트가 아닌 언어에서도 JSON을 충분히 다룰 수 있다.
+ 그래서 JSON을 데이터 교환 목적으로 사용하는 경우가 많다.

#### 자바스크립트가 제공하는 JSON 관련 메서드
+ `JSON.stringfy()`: JavaScript 객체를 JSON 문자열로 변환
+ `JSON.parse()`: JSON 문자열을 JavaScript 객체로 변환

#### JSON.parse()
+ parse() 메서드는 JSON 문자열을 인자로 받고 결과값으로 JavaScript 객체를 반환한다.
+ 예시
```node
const str = {
  "name": "홍길동",
  "age": 25,
  "married": false,
  "family": { "father": "홍판서", "mother": "춘섬" },
  "hobbies": ["독서", "도술"],
  "jobs": null
}

const obj = JSON.parse(str); //결과값을 obj라는 변수에 저장

console.log(obj); // JSON 문자열 형태의 데이터가 JavaScript 객체의 형태로 변환되어 출력

// 결과
{
    name: "홍길동",
    age: 25,
    married: false,
    family: {
        father: "홍판서",
        mother: "춘섬"
    },
    hobbies: [
        "독서",
        "도술"
    ],
    jobs: null
}
```
+ JSON 문자열에서는 `키(key)`를 나타낼 때 반드시 쌍따옴표로 감싸줘야 하는 반면에, `JavaScript` 객체에서는 쌍따옴표를 꼭 사용할 필요는 없자.
+ 단, 특수 문자나 영어 외의 언어와 같이 키로 허용되지 않는 문자를 키로 사용해야하는 경우에는 쌍따옴표를 사용해야 한다.

#### JSON.stringify()
+ JavaScript 객체를 JSON 문자열로 변환할 때는 JSON 객체의 `stringify()` 메서드를 사용한다.
+ stringify() 메서드는 JavaScript 객체를 인자로 받고 JSON 문자열을 반환한다.
+ 예시
  + parse() 메서드의 호출 결과로 얻은 JavaScript 객체를 obj이라는 변수에 저장한 것을 JSON.stringify() 메서드에 obj를 인자로 넘겨 호출
  ```node
  const str = JSON.stringify(obj);
  console.log(str);
  
  //결과
  '{"name":"홍길동","age":25,"married":false,"family":{"father":"홍판서","mother":"춘섬"},"hobbies":["독서","도술"],"jobs":null}'
  ```
#### 왜 이 메서드들을 사용할까??🧐
+ 자바스크립트는 근본적으로 깊은 복사가 불가능하다.
+ 1 depth 배열인 경우 slice로도 깊은 복사가 가능하다. 그러나 1 depth가 아닌 경우는 불가능하다.
+ 이를 위해 기본적으로 `const temp = JSON.parse(JSON.stringify(array));` 이런식으로 사용한다.
+ 참고📢
  + 보통 배열을 복사하는 경우는 그 배열의 값을 비교할 경우 사용한다.


