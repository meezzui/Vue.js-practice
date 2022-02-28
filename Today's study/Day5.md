### 오늘의 일기
---
#### HTML코드 데이터 바인딩 방법
+ 태그 안에 `v-html="리턴값"`넣어주기
```node
<template>
    <div v-html="htmlString"></div>  // html 데이터 바인딩
</template>
<script>
export default {
    name: 'dataBinding',
    components: {},
    data(){
        return{
            htmlString: '<p style="color:red;">This is a red string</p>' // 뻘간색 글씨로 결과 표시
        };
    }
```
+ 결과는 `This is a red string` 빨간색으로 나온다
+ 문자열 바인딩이랑 다른점은 `<p style="color:red;">This is a red string</p>`자체가 출력된다는 것이다.

#### 입력폼 데이터 바인딩 방법
+ input 태그 - type="text"
  + `v-model` : input태그에서 value와 같은 기능을 한다.
  ```node
  <template>
      <input type="text" v-model=" valueModel"/>
  </template>
  <script>
  export default {
      name: 'dataBinding',
      components: {},
      data(){
          return{
              valueModel: 'South Korea'
          };
      }
  ```
+ input 태그 - type="number"
  + `v-model.number` : value와 같은 기능으로 숫자 형태로 나타내준다.
  + data()안에 있는  변수의 객체를 console로 찍을 때는 `this.` 으로 시작해야 한다.
  + `mounted`안에 선언한 것은 `this.`으로 시작하지 않아도 된다.
  ```node
  <template>
    <input type="text" v-model=" valueModel"/> //value값 문자
    <input type="number" v-model.number="valueMode2"/> // value값 숫자
  </template>
  <script>
  export default {
      name: 'dataBindingInputText',
      components: {},
      data(){
          return{
              valueModel: 'South Korea',
              valueModel2: 12
          };
      },
      setup(){},
      created(){

      },
      mounted(){
        let valueModel3 = 11;
        console.log(this.valueMode2); 
        console.log(valueModel3);
      },
      unmounted(){

      },
      methods:{

      }
  }
  </script>
  ```
+ textarea 태그
  + 태그 안에 `v-model`을 이용하여 바인딩 해준다.
  + div 태그에 문자열을 바인딩 하는 것과는 엄연히 다르다.
  + `<textarea v-model="valueModel3"></textarea>`

    
    
    
    
    
    
    
