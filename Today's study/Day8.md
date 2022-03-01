### v-bind 간단하게 나타내기
+ 속성앞에 `:` 만 붙여주면 된다.
### 오늘의 일기
---
#### 리스트 랜더링
+ 리스트 랜더링이란 테이블을 이용해서 여러 데이터를 보여주는 경우, select박스의 option들 같은 경우 등 리스트들을 보여줄 때 랜더링하는 방법이다.
+ `v-for`를 이용한다.
+ `v-for="(city,i) in options"` : city는 options 배열안에 데이터들을 가리킨다.(3개니까 3번 돈다) i는 몇번째 인덱스인지를 가리킨다. 
+ `:key="i"`: key값을 잡아줘야 한다. key값은 인덱스 값 i로 지정하였다.
```node
<template>
    <div>
        <select v-model="city"> // 제주가 선택된 상태로 나온다.
            <option :value="city.v" :key="i" v-for="(city,i) in options">{{city.t}}</option>
        </select>
    </div>
</template>
<script>
export default {
    name: 'ListRendering',
    components: {},
    data(){
        return{
            options: [
                {v: '02', t:'서울'},
                {v: '031', t:'경기도'},
                {v: '064', t:'제주'}
            ],
            city: '064'
        };
    }
}
</script>
```
+ table 태그
  + `<td>`가 반복되니까 `<tr>`만큼 돌게 만든다고 생각하면 된다.
  ```node
  <template>
      <div>
          <table>
              <thead>
                  <tr>
                      <th>제품명</th>
                      <th>제품가격</th>
                      <th>배송비</th>
                      <th>제품카테고리</th>
                  </tr>
              </thead>
              <tbody>
                  <tr :key="i" v-for="(product, i) in productList">
                      <td>{{product.product_name}}</td> // 각각의 데이터들 넣어주기
                      <td>{{product.price}}</td>
                      <td>{{product.delivery_price}}</td>
                      <td>{{product.category}}</td>

                  </tr>
              </tbody>
          </table>
      </div>
  </template>
  <script>
  export default {
      name: 'ListRendering',
      components: {},
      data(){
          return{
              productList:[
                  {product_name:'기계식 키보드', price:25000,delivery_price:5000,category:'전자제품'},
                  {product_name:'마우스', price:20000,delivery_price:3000,category:'전자제품'},
                  {product_name:'콘센트', price:30000,delivery_price:4000,category:'전자제품'}
              ]
          };
      }
  }
  </script>
  ```
#### 랜더링 문법
+ `v-if`
  + if문과 같은 기능을 한다.
  + 아래 예시를 보면 type값에 따라 화면에 랜더링 되는게 달라진다. 현재 type값이 C이니까 화면에 보여지는 결과는 'C 보임'이다. 
  ```node
  <template>
      <div>
          <h1 v-if="type=='A'">A 보임</h1>
          <h1 v-else-if="type=='B'">B 보임</h1>
          <h1 v-else-if="type=='C'">C 보임</h1>
          <h1 v-else>Other</h1>
      </div>
  </template>

  <script>
  // @ is an alias to /src
  import HelloWorld from '@/components/HelloWorld.vue'

  export default {
      name: 'RenderingLogic',
      components: {},
      data(){
          return{
              type: 'C'
          }
      }

  }
  </script>
  ```
+ `v-show`
  + `v-if`와 같은 기능인데 else가 존재하지는 않음
  ```node
  <h1 v-show="bRender">A</h1> //bRender가 true이기 때문에 화면에 A라고 보여짐
  export default {
      name: 'RenderingLogic',
      components: {},
      data(){
          return{
              bRender: true
          }
      }

  }
  ```
 #### `v-if`와 `v-show`의 차이점
 + 랜더링 되는 방식에서 큰 차이가 있다.
 + `v-if`
   + 조건이 만족되면 그 순간에 html블럭이 생성이 되고 만족하지 않으면 html블록은 삭제가 된다.(관리자모드에서 봐도 html코드가 안 보임)
   + 한번 랜더링되고 끝나는 경우 사용
+ `v-show` 
  + 조건이 만족되지 않았을 때 html블록이 삭제가 되는 것이 아니라 style속성의 display:none으로 가려져서 안 보이는 것이다.(관리자모드에서 보면 html코드가 보임)
  + 한 화면에서 버튼을 누를 때 빈번히 보였다 안 보였다를 처리할 경우 사용
  













