### <script setup>이란?
+ Vue 3에서는 컴포넌트 객체에 `setup` 함수를 사용할 수 있다.
+ `ref` 를 사용해서 반응형 변수로 `data`를, 보통의 자바스크립트 함수로 `methods`를 대체한다. 
+ 컴포넌트 객체에서 속성으로 분리되었던 기능 대부분이 `setup` 함수 안에서 사용 가능하다. 
+ 여기서 중요한점!!!🎉 `setup 함수는 컴포넌트 인스턴스가 생성되기 전에 실행된다.`
  + 즉, 컴포넌트 인스턴스에 접근이 필요한 기능은 사용할 수 없다. 
  + 다시말해, `setup` 안에서 `this`를 통해 컴포넌트 객체의 `data, computed, methods` 에서 선언한 것들에는 접근이 불가능하다.

### <script setup> 페이지 적용법
+ 원래 컴포넌트 객체의 setup 함수는 data, methods 와 마찬가지로 return 을 사용해서 템플릿으로 값을 넘기는 과정이 필요하다. 하지만 Vue 3에서는 이런 불편함을 해소할 수 있는 방법을 제공한다.
+ `return`을 통해서 `template`에 명시적으로 전달하는 과정이 생략된다.
+ 컴포넌트 모듈도 `import`만 해주면 `template` 안에서 사용할 수 있다.
+ `setup()` 함수 에서 반환된 값과 유사하게 `ref`는 템플릿에서 참조될 때 자동으로 래핑 해제된다.
  + 예시
  ```node
  <script setup>
  import { ref } from 'vue'

  const count = ref(0)
  </script>

  <template>
    <button @click="count++">{{ count }}</button>
  </template>
  ```
#### Alert 기능 만들기
+ commponents로 따로 빼서 해준다.
+ `Alert.vue`
  + script부분을 보면
  ```node
  <script setup>
  import { defineExpose, ref } from 'vue'

  const show = ref(false) //보여주는 여부
  const option = ref({}) // alert에 들어가는 틀 옵션
  const onClose = ref(() => {}) // 닫기 기능(alert 창 꺼지기)
  const onSuccess = ref(() => {}) // 확인버튼

  defineExpose({ // 이렇게 해줘야 변수를 사용할 수 있음
    option,
    show,
    onClose,
    onSuccess
  })
  </script>
  ```
+ `<div v-show="show" id="alert_mobile">`: `v-show`로 화면단에 보여준다.
+ `<img v-if="option.img" :src="require(`@/assets/image/icon-alert-${option.img}.png`)" alt="" class="img">` : 이미지 들어가는 공간
  + 🎗 :src="require(`@/assets/image/icon-alert-${option.img}.png`)" 🎗 : img 경로를 적고 이미지 파일 이름을 `icon-alert-`이걸로 시작해야 한다. 그러면 사용자단에서는 이 뒷부분 이름만 가지고 사용하면 된다.
+ `<p class="title" v-html="option.title"/>` : 이미지 아래 텍스트 영역
+ `<p v-if="option.msg" class="msg" v-html="option.msg"/>` : 그 텍스트 영역 아래 더 작은 글씨로 메시지 영역
+ `<button v-if="!option.right" role="button" class="button" @click="onSuccess()">{{ option.single ? option.single : 'Confirm' }}</button>`
  + 버튼의 오른쪽 부분을 의미하며 `onSuccess()`함수가 실행된다.
  + `{{ option.single ? option.single : 'Confirm' }}`: single버튼이 아니면 확인버튼 기능(사용자단에서 <BottomButton>태그에 single="true"를 쓰면 생김)
+ `<div v-if="option.right" role="button" class="two-button">` : 버튼이 두개인 것을 만들때 보여진다.(사용자단에서 <BottomButton>태그에 left,right를 쓰면 생김)
+ `Alert.vue` 전체 
```node
<template>
  <div v-show="show" id="alert_mobile">
    <section class="popup_view">
      <div class="contents">
        <img v-if="option.img" :src="require(`@/assets/image/icon-alert-${option.img}.png`)" alt="" class="img">
        <p class="title" v-html="option.title"/>
        <p v-if="option.msg" class="msg" v-html="option.msg"/>
      </div>
      <div class="button_wrap">
        <button v-if="!option.right" role="button" class="button" @click="onSuccess()">{{ option.single ? option.single : 'Confirm' }}</button>
        <div v-if="option.right" role="button" class="two-button">
          <button @click="onClose()">{{ option.left ? option.left : 'Cancel' }}</button>
          <button @click="onSuccess()">{{ option.right }}</button>
        </div>
      </div>
    </section>
  </div>
</template>

<style lang="scss" scoped>
#alert_mobile {
  position: fixed; top: 0; left: 0;width: 100vw; height: 100vh; background: rgba($color: #000000, $alpha: 0.4); z-index: 9999; padding: 0 16px;
  @include center;
 .popup_view{
   width: 320px; background: white;position:relative; margin: 0 auto; text-align: center;
    .contents{
      width: 100%; height: max-content; padding: 30px;
      img {
        width: 36px; margin-bottom: 6px;
      }
      p {
        word-break: break-all;
      }
      .title {
        font-size: 14px;
      }
      .msg {
        font-size: 12px; margin-top: 6px; color: $txt-d-gray;
      }
    }
    .button_wrap {
      display: flex; width: 100%; height: 47px;border-top: 1px solid #EBEBEB;
      button {
        flex: 1; font-size: 14px;
      }
      .two-button {
        display: flex; width: 100%; align-items: center;
        button {
          height: max-content;
          &:nth-child(1) {
            color: $txt-d-gray;
          }
        }
      }
    }
 }
}
</style>

<script setup>
import { defineExpose, ref } from 'vue'

const show = ref(false)
const option = ref({})
const onClose = ref(() => {})
const onSuccess = ref(() => {})

defineExpose({
  option,
  show,
  onClose,
  onSuccess
})

</script>
```
+ `Alert.js`
```node
import AlertComponent from './Alert.vue'
import { createApp } from 'vue'

const Alert = {}

Alert.install = (app) => {
  // const AlertConstructor = { extend: AlertComponent }
  const tempDiv = document.createElement('div')
  const instance = createApp(AlertComponent).mount(tempDiv)

  document.body.appendChild(instance.$el)

  app.config.globalProperties.$alert = (option) => {
    instance.show = true
    // alert logic
    instance.option = option
    instance.onClose = () => {
      instance.show = false
      if (option.close) {
        option.close()
      }
    }
    instance.onSuccess = () => {
      instance.show = false
      if (option.success) {
        option.success()
      }
    }
  }
}

export default Alert
```
+ commponets를 이제 실제 사용단에 적용하는 방법
```node
<div>
  <BottomButton heart=true right="구매" isValid="false" isPadding= "true" @click-right="clickedPurchase()"/>
</div>
        
methods: {
  clickedPurchase() {
    this.$alert({
      title: '장바구니에 상품이 담겼습니다.',
      left: '계속 쇼핑',
      right: '장바구니 이동',
      img: 'bascket'
    })
  }
}
```
+ 구매 버튼을 클릭했을 때 alert 창이 뜨게 하기 위한 코드이다.
+ 해당 태그에 `@click-right="clickedPurchase()"`라는 클릭 이벤틀를 걸어준다. 여기서 `@click`이 아닌  `@click-right`이렇게 해준 이유는 '구매'버튼이 오른쪽에 있기 때문이다.
+ `clickedPurchase()` : 이 메소드의 요소들의 해당 이름들은 Alert.vue 에서 사용되었던 이름들이다. 사용할 때는 그것들의 기능에 맞게 적어주면 된다.
 
