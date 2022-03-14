### 오늘의 일기
---
#### `<template>`을 따로 나누지 않고 탭 메뉴 선택시 css만 바뀌게 하기
+ 범위 css를 사용하여 부모와 자식관계의 형태로 css를 사용할 수 있다.
```node
<div class="coupon-container">
  <ul class="coupon-box">
    <li v-for="(items, i) in [1,1,1]" :key="i" :class="isUse ? '' : 'disabled'">
      <div class="coupon-innerbox">
        <div class="coupon-innertag1">
          <span>10% 할인쿠폰</span>
        </div>
        <div class="coupon-innertag2">
          <span>20.000원 이상 주문 시 최대 5,000원 할인</span>
        </div>
        <div class="coupon-innertag3">
          <span>2021.11.30까지</span>
        </div>
      </div>
    </li>
  </ul>
</div>
 
<style lang="scss" scoped>
 // .coupon-box 자식인 .disabled 아래의 .coupon-innerbox와 .coupon-innertag3 클래스의 특정 css속성만 변경 
.coupon-box{
    .disabled{
      .coupon-innerbox{
         border-left: solid #D1D3D7;
      }
      .coupon-innertag3{
        color: #D1D3D7;
      }
   }
}
</style>
 
<script>
data() {
  return {
    title: '쿠폰',
    isUse: true
  }
</script>
```
+ 결과 : 미사용을 누르면(true적용) `border-left`와 `coupon-innertag3`의 색이 `#22BDB6`으로 보이고 사용 완료/만료를 누르면 `#D1D3D7`으로 바뀐다.
+ `:class="isUse ? '' : 'disabled'"`: `isUse`가 `false`이면 `disabled` 클래스이름의 스타일이 적용된다.(true는 빈 값이 전달된다.)
  
  
  
  
  
  
  
  
