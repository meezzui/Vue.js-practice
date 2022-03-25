### 버튼 클릭시 알림창 떴다 사라지게 타이머 걸기
+ '쿠폰 받기'를 클릭하면 '쿠폰 발급'이 '완료'되었다는 알림이 하단에 나타났다가 사라지는 기능 구현
```node
<template>
----------(생략)----------

<div @click="clickedDownLoad()">
  <p>쿠폰받기</p>
</div>
----------(생략)----------

<div class="coupon-message-contain">
  <div class="coupon-message-box" :class="isDownLoad ? 'on' : ''">
    <div>
      <img src="@/assets/image/temp/complete.png">
    </div>
    <div>
      <p>쿠폰 발급이 완료되었습니다.</p>
    </div>
  </div>
</div>
</template>

<style lang="scss" scoped>
.coupon-message-box{
  display: flex;
  border: 1px solid #22BDB6;
  border-radius: 20px;
  background: linear-gradient(to right, #22BDB6,#2EB5C6,#3AADD6,#44A6E4,#4BA0EE,#509DF5);
  opacity: 0;
  height: 32px;
  align-items: center;
  vertical-align: middle;
  padding: 7px 84px;
  position: fixed;
  bottom: 10px;
  transition: opacity 1s ease-in-out;
  div:first-child{
    margin-right: 6.4px;
  }
  div{
    img{
      width: 10.08px;
      height: 6.45px;
    }
    p{
      font-size: 12px;
      color: #FFFFFF;
    }
  }
}
.on {
  opacity: 80%;
}

<script>
import { IonPage } from '@ionic/vue'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'DownLoadCouponPage',
  components: {
    IonPage
  },
  data() {
    return {
      title: '다운 가능 쿠폰',
      isDownLoad: false
    }
  },
  created() {
  },
  methods: {
    clickedDownLoad() {
      this.isDownLoad = true
      setTimeout(() => {
        this.isDownLoad = false
      }, 2000)
    }
  }
})
</script>
```
+ `data(){return{isDownLoad: false }}`: `isDownLoad`의 기본값은 false로 지정.
+ `@click="clickedDownLoad()"` : 그러나 쿠폰받기를 눌렀을 시 `clickedDownLoad()` 함수 실행 -> `this.isDownLoad = true` 이 코드로 인해 `true`로 변경 됨
+ 그리고 `setTimeout()`이라는 뷰 문서에 적용되어 있는 함수를 이용하여 `isDownLoad`의 값이 `false`로 변하는데 2초가 걸리게 함. 
+ `:class="isDownLoad ? 'on' : ''"`: `isDownLoad`가 `true`이면 `.on{}` 안에 css가 적용되고 `false`이면 빈값을 보냄
+ `.coupon-message-box`클래스의 css를 보면 `opacity: 0;`으로 기본상태에선 '쿠폰 발급 완료' 메시지가 안 보이게 했다. 
+ '쿠폰 받기'를 누르면 `.on`클래스의 css가 실행되면서 `opacity:80%`가 적용되고 `.coupon-message-box`클래스의 css에 `transition: opacity 1s ease-in-out;`이 효과로 인해 1초 동안 서서히 사라지게 보이는 애니메이션 효과를 줄 수 있다.   





