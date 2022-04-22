#### input태그 value 값 데이터 보내기
+ 먼저 데이터 부분에 `v-model` 할 변수 선언
+ `v-model="checkType"` : 각각 input태그에 똑같은 `v-model` 값을 걸어준다.
+ `value="V"`, `value="D"` : 각각의 value 값을 준다.
+ `@change="setTotalPrice"` : `@change` 이벤트로 라디오버튼이 체크되었을때 실행될 함수를 적어준다.(주의❗❗❗ `@click` 이벤트를 쓸 경우 통신이 원활하지 않을 수 있으므로 `@change` 이벤트를 쓴다.)
```node
<template>
  <input v-model="checkType" v-if="productDetail.frcsPrdtTp === 'C'" @change="setTotalPrice" type="radio" value="V" name="selectradio" id="trade">
  <input v-model="checkType" v-if="productDetail.frcsPrdtTp === 'C'" @change="setTotalPrice" type="radio" value="D" name="selectradio" id="handsfree">
</template>
data() {
  return {
    checkType: ''
  }
},
setTotalPrice() {
  console.log('radio', this.checkType)
  this.totalPrice = 0

  for (let i = 0; i < this.orderOpt.length; i++) {
    this.totalPrice = this.totalPrice + (this.orderOpt[i].cnt * this.productDetail.frcsSalAmt)
  }
  if (this.checkType === 'D') { // 핸즈프리 - 배송비 포함
    this.totalPrice += this.productDetail.dlvyAmt
  }
}
```
