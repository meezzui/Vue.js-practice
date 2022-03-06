### 오늘의 일기
---
#### 부모와 자식 컴포넌트 관계
+ 부모에서 자식 컴포넌트로 데이터 전송
  + `props` 사용(단방향 데이터 전달 방식)
    + 부모 컴포넌트에서 자식 컴포넌트를 호출시 자식 컴포넌트 태그 내 v-bind나 : 키워드를통해 데이터를 전달하고 자식 컴포넌트에서 props객체를 통해 데이터를 전달받는 방식이다.
  + 기본형태
  ```node
  <!--부모 컴포넌트-->
  <template>
      <!--첫번째, 두번째 모두 같은 결과-->
      <!--첫번째 방법-->
      <자식컴포넌트이름 :props이름="전달데이터"/>
      <!--두번째 방법-->
      <자식컴포넌트이름 v-bind:props이름="전달데이터"/>
  </template>
  ```
 + 부모 컴포넌트에서 자식 컴포넌트로 이벤트 발생시키기
   + 자식컴포넌트에서 발생한 이벤트를 부모 컴포넌트에서 실행시키는 것이다.
   + `ref=""`사용
   + 자식 컴포넌트(ChildComponent)
   ```node
   <div>
    <button type="button" @click="childFunc" ref="childbtn">자식에 있는 클릭</button>
   <div>
   
   <script>
    methods:{
      childFunc(){
        alert('부모 컴포넌트에서 직접 발생시킨 이벤트');
      }
    }
    </script>
   ```
   + 부모 컴포넌트
   ```node
   <template>
   <div>
    <button type="button" @click="callChildFunc" ref="childbtn">부모에 있는 클릭</button>
    <ChildComponent ref="child_component"/>
   <div>
   <template>
   <script>
   import ChildComponent from './ChildComponent';
   
   export default{
    name: '',
    components: {ChildComponent},
    data(){
      return{
      
      };
    },
    methods:{
      callChildFunc(){
        this.$refs.child_component.$refs.child_btn.click(); //this.$refs.child_component : <ChildComponent ref="child_component"/>를 접근
                                                  //.$refs.child_btn : childComponent의 child_btn까지 접근
      }
    }
   }
   </script>
   ```
   + 그러면 부모에 있는 클릭을 클릭하는 순간 refs를 통해서 child_component에 접근을 하고 또 refs를 통해서 child_btn을 접근함.
   + `부모 컴포넌트에서 직접 발생시킨 이벤트` 알림이 뜸
   
   
