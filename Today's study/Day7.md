### 오늘의 일기
---
#### 속성에 데이터 바인딩 하기
+ 이미지 태그
  + `v-bind`로 url값 넣기
  ```node
  <img v-bind:src="url" style="height:200px;width:auto;"/>
  
  export default {
    name: 'Example2',
    components: {},
    data(){
        return{
            url : '../src/assets/logo.png'
        };
    }
  ```
  
+ 버튼 태그
  + 버튼 태그의 disabled속성을 이용하여 값이 입력되어야만 버튼이 활성화 되게 하기(실무에서 많이 사용됨)
  ```node
  <input type="text" v-model="textValue"/>
  <button type="button" v-bind:disabled="textValue==''">Click</button>  // v-bind:disabled="textValue==''" : input 태그에 값이 입력되지 않으면 비활성화
  
  export default {
    name: 'Example2',
    components: {},
    data(){
        return{
            textValue:''
        };
    }
  
  ```

#### 클래스와 스타일에 데이터 바인딩하기
+ 클래스를 이용하여 스타일 데이터 바인딩
  + `v-bind:class="{}"`: 기존 class에 add 시킨다고 생각하면 됨
  + 결과 : `.container{width:100%; height: 200px;}` 와 `.active{background-color: yellow; font-weight: bold;}` 클래스의 스타일 속성만 적용됨
  ```node
  <template>
      <div class="container" v-bind:class="{'active':isActive,'text-red':isRed}">Class Binding</div> //기존 container 클래스에 active와 text-red 클래스가 추가됨
  </template>
  <script>
  export default {
      name: 'Example3',
      components: {},
      data(){
          return{
              isActive:true, // 스타일이 적용됨
              isRed: false // 스타일이 적용되지 않음
          };
      }
  }
  </script>
  <style scoped>

  .container{width:100%; height: 200px;}
  .active{background-color: yellow; font-weight: bold;}
  .text-red{ color:red;}

  </style>
  ```
+ 배열을 이용하여 스타일 바인딩
  + `v-bind:class="[]"` : 대괄호 안에 지정한 이름의 클래스들이 기존의 클래스에 추가 됨
  + 결과 : `.container{width:100%; height: 200px;}`, `.active{background-color: yellow; font-weight: bold;}`, `.text-red{ color:red;}` 스타일이 모두 적용됨
  ```node
  <template>
      <div class="container" v-bind:class="[activeClass, redClass]">Class Binding</div> // container라는 클래스에 activeClass, redClass가 추가 됨
  </template>
  <script>
  export default {
      name: 'Example3',
      components: {},
      data(){
          return{
              activeClass:'active', // style태그 안에 쓴 클래스 이름 적기
              redClass: 'text-red' // style태그 안에 쓴 클래스 이름 적기
          };
      }
  }
  </script>
  <style scoped>

  .container{width:100%; height: 200px;}
  .active{background-color: yellow; font-weight: bold;}
  .text-red{ color:red;}

  </style>
  ```
  
+ 인라인 스타일 바인딩
  + 인라인 스타일은 카멜 표기법을 사용한다.
  ```node
  <div v-bind:style="styleObject">인라인 스타일 바인딩</div>
  
  export default {
      name: 'Example3',
      components: {},
      data(){
          return{
              styleObject:{
                  backgroundColor:'pink',
                  color:'red',
                  fontWeight:'bold'
              }
          };
      }
  }
  ```
+ 배열을 이용한 인라인 스타일 바인딩
  + ` v-bind:style="[]"`: 대괄호 안에 바인딩할 이름 넣어주기
  ```node
   <div v-bind:style="[baseStyle,addStyle]">인라인 스타일 바인딩</div>

   export default {
        name: 'Example3',
        components: {},
        data(){
            return{
                baseStyle: 'background-color:orange;',
                addStyle: 'color:red; font-weight:bold'
            };
        }
    }
  ```













