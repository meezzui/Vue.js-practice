### 오늘의 일기
---
#### 입력폼 데이터 바인딩
+ select
  + `v-model`은 value와 같은 기능을 한다.(value의 속성과 데이터 바인딩이 된다.)
  ```node
  <select v-model="city">
        <option value="02">서울</option>
        <option value="21">부산</option>
        <option value="031">경기도</option>
    </select>
    
    <script>
    export default {
        name: 'dataBindingInputText',
        components: {},
        data(){
            return{
                city: "031" //경기도가 선택된 상태로 보인다.
            };
    </script>
    ```
+ check 박스
  + check 박스의 `v-model`은 value가 아니라 checked 속성과 연결이 된다.
  + checked를 true로 해 놓으면 체크박스가 체크되어져 보이고 false로 해놓으면 체크되어 있지 않는다.
  + 그리고 체크박스를 누르면 자동으로 그 반대 속성으로 변한다.
  ```node
  <label><input type="checkbox" v-model="checked">{{checked}}</label> //{{checked}} : checked 값이 보임. 체크를 해제시 같이 false로 바뀜
 
  <script>
  export default {
      name: 'dataBindingInputText',
      components: {},
      data(){
          return{
              checked : true // 체크되어  보임. flase면 체크 안됨
          };
      }
   <script>
  ```
+ check 박스가 여러개인 경우
  + 배열을 사용한다.
  ```node
  <label><input type="checkbox" value="서울" v-model="checked">서울</label>
  <label><input type="checkbox" value="부산" v-model="checked">부산</label>
  <label><input type="checkbox" value="제주도" v-model="checked">제주도</label>

  <br>
  <span>체크한 지역: {{checked}}</span>
  
  <script>
  export default {
      name: 'dataBindingInputText',
      components: {},
      data(){
          return{
              checked : [] // 배열을 사용하면 체크박스가 여러개 있을 때 체크한 것들이 여기에 담긴다.
          };
      }
  <script>
  ```
    
+ radio 박스
  + `v-model`은 value가 아니라 checked 속성과 연결이 된다.
  + value에 데이터 바인딩을 하고 싶을 때는 `v-bind:`를 속성 앞에 넣어주면 데이터 바인딩을 할 수 있다.
  ```node
  <label><input type="radio" v-bind:value ="radio1" v-model="picked"/>서울</label>
  <label><input type="radio" v-bind:value ="radio2" v-model="picked"/>부산</label>
  <label><input type="radio" v-bind:value ="radio3" v-model="picked"/>제주</label>
  
  <script>
    export default {
        name: 'dataBindingInputText',
        components: {},
        data(){
            return{
              picked : "",
              radio1: "서울",
              radio2: "부산",
              radio3: "제주",

            };
        }
   <script>
   ```
   + 배열을 이용한 바인딩(value 값으로 배열위치를 넣어준다.)
   ```node
   <label><input type="radio" v-bind:value ="radio[0]" v-model="picked"/>서울</label>
   <label><input type="radio" v-bind:value ="radio[1]" v-model="picked"/>부산</label>
   <label><input type="radio" v-bind:value ="radio[2]" v-model="picked"/>제주</label>
     
   <script>
    export default {
        name: 'dataBindingInputText',
        components: {},
        data(){
            return{
              picked : "",
              radio: ["서울", "부산","제주"]
            };
        }
   <script>
   ```
   
   
    
     
 















