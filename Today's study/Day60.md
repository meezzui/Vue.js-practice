#### `props`로 넘겨 받은 header에 back버튼, close버튼 클릭 시 원하는 페이지로 이동
+ back버튼을 눌렀을 시 원하는 페이지로 이동을 해야한다.
+ 예를들어, 비밀번호 변경 페이지에서 back버튼을 눌렀는데 마이페이지로 연결되지 않고 그 전 페이지로 이동되는 현상이다. 
+ 이를 해결하기 위해 해당 페이지별로 버튼의 이동경로를 다르게 해줘야 한다. 
+ `header`는 따로 컴포넌트 파일로 빼서 `props`로 헤더의 버튼 기능들을 사용하고 있기 때문에 이는 `header` 파일에서 먼저 잡아주어야 한다.
  + 즉, props에 변수를 하나 더 선언해주어야 한다.
  + 그리고 버튼 메소드도 `router`가 해당 `path`의 `name`으로 탈 수 있게 끔 변경해주어야 한다.
+ 💕`headerOption`💕 : `props`로 넘기는 변수
```node
// 뒤로가기 버튼
<div v-if="headerOption.back" class="back-button-wrapper">
  <img src="@/assets/image/icon-back.png" alt="back button"
  @click="headerOption.routeTo ? handleBackbutton() : $router.go(-1)">
</div>

// 닫기 
<ion-button v-if="headerOption.close" @click="handleBackbutton()"><img :src="headerOption.backcolor ? require('@/assets/image/icon-x-w.png') : require('@/assets/image/icon-x.png')" class="close icon" alt=""></ion-button>

methods: {
  handleBackbutton() {
    if (this.headerOption.routeTo) { // 만약 headerOption.routeTo라면 
      this.$router.push({ name: this.headerOption.routeTo }) // path의 name으로 해당 페이지로 이동한다.
    } else { // 그렇지 않을 경우
      this.headerOption.isModal ? this.$emit('close') : this.$router.go(-1) // 모달창일 경우 close버튼을 누르면 모달창이 없어지고 모달창이 아닌 경우는 이전 페이지로 이동한다.
    }
  }
}
```
+ `@click="headerOption.routeTo ? handleBackbutton() : $router.go(-1)"` : `headerOption`의 routeTo가 true(사용하는 페이지에서 `routeTo`라고 쓰면 true인 것임)이면 `handleBackbutton()`이 함수를 타고 그렇지 않으면 이전 페이지로 이동한다.
+ `<ion-button v-if="headerOption.close" @click="handleBackbutton()">`: 사용하는 페이지에서 `close:'true'`이렇게 사용한다. 이렇게 하면 해당 페이지에 x 이미지가 보인다. 그리고 그것을 클릭하면 `handleBackbutton()`이 함수를 탄다.
#### header 전체 코드
```node
<template>
  <ion-header id="header-container">
    <ion-toolbar :class="headerClass">
      <ion-buttons slot="start">
        <div v-if="headerOption.back" class="back-button-wrapper">
          <!-- <ion-back-button mode="md" class="icon"></ion-back-button> -->
          <img src="@/assets/image/icon-back.png" alt="back button"
          @click="headerOption.routeTo ? handleBackbutton() : $router.go(-1)">
        </div>
        <div v-if="headerOption.menu"><ion-menu-button mode="md" auto-hide="false"></ion-menu-button></div>
      </ion-buttons>
      <p v-if="headerOption.title" class="title">{{headerOption.title}}</p>
      <ion-buttons slot="end">
        <ion-button v-if="headerOption.search" @click="routeTo('Search')"><img src="@/assets/image/icon-search.png" class="search icon" alt=""></ion-button>
        <ion-button v-if="headerOption.close" @click="handleBackbutton()"><img :src="headerOption.backcolor ? require('@/assets/image/icon-x-w.png') : require('@/assets/image/icon-x.png')" class="close icon" alt=""></ion-button>
        <ion-button v-if="headerOption.alert"><img src="@/assets/image/icon-alert.png" class="alert icon" alt="" @click="routeTo('NoticeCenter')"></ion-button>
        <ion-button v-if="headerOption.cart" @click="routeTo('OrderCart')" :class="cartCnt > 0 ? 'cart-wrapper': 'cart-wrapper-off'"><img src="@/assets/image/icon-cart.png" class="cart icon" alt=""></ion-button>
        <ion-button v-if="headerOption.home" @click="routeTo('Main')"><img src="@/assets/image/home-off.png" class="home icon" alt=""></ion-button>
      </ion-buttons>
    </ion-toolbar>
  </ion-header>
</template>

<style lang="scss" scoped>
  =====================================(생략)======================================
</style>

<script>
import {
  IonHeader,
  IonToolbar,
  IonButtons,
  IonMenuButton,
  IonButton
  // IonBackButton
} from '@ionic/vue'
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'HeaderBar',
  props: ['headerOption', 'is-scroll'],
  components: {
    IonHeader,
    IonToolbar,
    IonButtons,
    IonMenuButton,
    IonButton
    // IonBackButton
  },

  setup() {
    return {

    }
  },
  data() {
    return {
      cartCnt: 0

    }
  },
  computed: {
    headerClass() {
      return `${this.headerOption.backcolor} ${this.isScroll ? 'scolled_border' : ''}`
    }
  },
  async created() {
    this.cartCnt = await this.$getAppDataStr('cartCnt')
  },
  async ionViewWillEnter() {
    this.cartCnt = await this.$getAppDataStr('cartCnt')
  },
  methods: {
    handleBackbutton() {
      if (this.headerOption.routeTo) {
        this.$router.push({ name: this.headerOption.routeTo })
      } else {
        this.headerOption.isModal ? this.$emit('close') : this.$router.go(-1)
      }
    }
  }
})
</script>
```
