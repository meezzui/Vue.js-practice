#### 공통 버튼 components에 따로 파일을 빼기
+ components > BottomButton.vue
```node
<template>
  <section id="bottom-button-wrap" :class="isPadding ? 'padding-wrap' : ''">
    <button  v-if="single" class="bottom_buttom"  @click.prevent="$emit('click-single')"  :class="isValid ? 'on' : 'off'">
        {{single}}
    </button>
    <div v-else class="bottom_two_buttom" :class="generalBtns ? 'general' : ''">
      <button v-if="left" class="left" @click="$emit('click-left')">
        {{left}}
      </button>
      <button v-if="heart" class="heart" @click.prevent="handleLikeBtn">
        <input v-model="likeStatus" type="checkbox">
      </button>
      <button v-if="right" class="right" @click.prevent="$emit('click-right')" :class="isValid ? 'on' : 'off'">
        {{right}}
      </button>
    </div>
  </section>
</template>

<style lang="scss" scoped>
#bottom-button-wrap {
  position: fixed; bottom: 0; right: 0; width: 100%; background: white;

  .bottom_buttom {
      font-size: 16px;  color: #fff!important; width: 100%;
      height: calc(env(safe-area-inset-bottom) + 48px);
      height: calc(constant(safe-area-inset-bottom) + 48px);
  }
  .bottom_two_buttom {
    display: flex;
      height: calc(env(safe-area-inset-bottom) + 48px);
  height: calc(constant(safe-area-inset-bottom) + 48px);
    .left, .right {
      font-size: 14px; flex: 1;
    }
    .left{
      color: $txt-d-gray; border-top: 1px solid #D1D3D7;
    }
    .heart {
       margin-right: 20px;  width: 22px; position: relative;
      input {
        all: unset;  width: 22px; height: 18px;
        // <img src="@/assets/image/icon-heart-off.png" alt="">
        &::after {
          content: ''; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 22px; height: 18px;
          @include background('~@/assets/image/icon-heart-off.png', 100% 100%, 50% 50%);
        }
        &:checked {
          &::after {
            @include background('~@/assets/image/icon-heart-on.png', 100% 100%, 50% 50%);
          }
        }
      }
    }
  }
  .general {
    border-top: 1px solid #EBEBEB;
    button {
      background: white!important; color: $txt-d-gray; border: 0!important;
      &:nth-child(1) {
        border-right: 1px solid #EBEBEB!important;
      }
    }
  }
  .on {
     background-color: $primary; color: white;
  }
  .off {
    background-color: $d-gray; color: white;
  }
}
.full-wrap {

}
.padding-wrap {
  padding: 10px 16px; border-top: 1px solid $border;
  button {
    border-radius: 6px; border: 0!important;
  }
  .left {
    border: 1px solid #D1D3D7!important; margin-right: 10px;
  }
}
</style>

<script>
import {
} from '@ionic/vue'
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'BottomButton',
  components: {
  },
  props: ['isPadding', 'single', 'isValid', 'left', 'right', 'generalBtns', 'heart'],
  setup() {
    return {

    }
  },
  data() {
    return {
      likeStatus: false
    }
  },
  created() {
  },
  methods: {
    handleLikeBtn() {
      this.$emit('like', this.likeStatus)
    }
  }
})
</script>
```
+ single

![image](https://user-images.githubusercontent.com/86812098/161500684-6d56fa88-7e02-4183-937b-3367e6eb7ef2.png)

+ two_button
  + views에서 사용할 때
  ```node
  //예1
  <div>
    <BottomButton left="취소" right="확인" :isValid="buttonValidation"/>
  </div>
  
  //예2 (왼쪽은 하트버튼-좋아요 기능)
  <div>
    <BottomButton heart=true right="구매" isValid="false" isPadding= "true" @click-right="clickedPurchase()"/>
  </div>
  ```
  ![image](https://user-images.githubusercontent.com/86812098/161500878-ad22470f-c2f0-48bb-8515-ce66abfd33bc.png)

+ padding이 들어간 버튼

![image](https://user-images.githubusercontent.com/86812098/161500954-4ba113ee-6700-4d21-97af-ff2fdfeef417.png)
