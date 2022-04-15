#### Event Emit -`$emit()`
+ 컴포넌트의 통신 방법 중 하위 컴포넌트에서 상위 컴포넌트로 통신하는 방식이다.
+ 다른 컴포넌트에게 이벤트를 전달하기 위해 사용한다.
+ 즉, 하위 컴포넌트에 $emit()을 쓰면 상위(진짜 보여지는 단-views파일)에서 그걸 바인딩으로 가져다 쓰면 그 이벤트가 하위 컴포넌트로 호출된다는 것!!!🎃
+ 예시
```node
// 자식 컴포넌트
<template>
  <div class="yellow lighten-3 pa-3">
    <h3>회원 정보 수정</h3>
    <p>수정내용</p>
    <v-text-field label="이름" v-model="user.name"></v-text-field>
    <v-text-field label="주소" v-model="user.address"></v-text-field>
    <v-text-field label="전화번호" v-model="user.phone"></v-text-field>
    <v-radio-group v-model="user.hasDog">
      <v-radio label="`반려견유`" :value="true"></v-radio>
      <v-radio label="`반려견무`" :value="false"></v-radio>
    </v-radio-group>
    <v-btn color="info" @click="update">수정</v-btn>
  </div>
</template>

<script>
export default {
  // 부모에게 받은 값을 바로 가공하면 에러발생. 그래서 props로 넘겨줘야 한다!!!
   props:['name','address','phone','hasDog'],
    data() {
      return{
        user:{}
      }
    // 부모 컴포넌트에서 props로 받은 값을 자식컴포넌트로 바꾼 뒤 사용하면 에러가 발생하지 않는다. 
// 위 data()안에 있는 user에 담기.
    },created(){
      this.user.name = this.name,
      this.user.address = this.address,
      this.user.phone = this.phone,
      this.user.hasDog = this.hasDog
   },
   methods:{
     update(){
      // 자식 컴포넌트에서 부모 컴포넌트로 보내는 것이 $emit()
      this.$emit("child",this.user)
    }
   }
}
</script>

=======================================================================================
// 부모 컴포넌트
<v-flex xs12 sm6>
       <UserEdit  
        :name="name"
        :address="address"
        :phone="phone"
        :hasDog="hasDog"
        @child="parents"></UserEdit>
      </v-flex>

<script>
export default {
methods:{
    parents(user){
      this.name = user.name,
      this.address = user.address,
      this.phone = user.phone,
      this.hasDog = user.hasDog
    }
  }
}
</script>
```
+ `this.$emit("child",this.user)` 이렇게 부모 컴포넌트로 값을 보냈으니까 `@child = "parents"` 메소드를 통해서 받은 값으로 부모 컴포넌트 값에 적용할 수 있다.
