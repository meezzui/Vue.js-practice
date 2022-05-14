#### props로 button custom해서 넘기기
+ `commponent` 파일에 공통으로 빼둔 `BottomButton.vue` 에서 `props`를 통해 custom한 버튼을 받아서 사용!!
+ 먼저 공통으로 빼둔 `BottomButton.vue`의 코드를 봐보자😃
  + 버튼은 single버튼과 two_button이 있다.
  + `<section id="bottom-button-wrap" :class="isPadding ? 'padding-wrap' : ''">`: 버튼에 padding 주기 => 사용 페이지에서 `isPadding`으로 버튼에 padding 유무를 정할 수 있다.
  + `<button  v-if="single" class="bottom_buttom"  @click.prevent="$emit('click-single')"  :class="isValid ? 'on' : 'off'">`
    + `v-if="single"`: single일 때만 보임
    +  `@click.prevent="$emit('click-single')"`: 버튼 클릭 이벤트 => 사용 페이지에서 `click-single="함수이름()"`이런 형식으로 사용하여 해당 버튼을 클릭 시 해당 함수가 실행되게 할 수 있다.
    + `:class="isValid ? 'on' : 'off'"`: `isValid`가 true이면 .on 클래스의 css가 적용, false이면 .off 클래스의 css가 적용된다. 이것은 체크박스에 체크가 되거나/input태그에 입력값이 입력되어야 버튼이 활성화되게 할 때 사용페이지에서 바인딩하여 사용한다. => `:isValid=""`
  + `{{single}}`: single버튼이 props를 통해 보이게 하기 위해 `{{}}`이걸로 묶어서 나타냄
  + `<div v-else class="bottom_two_buttom" :class="[`${generalBtns ? 'general' : ''}`, `${device === 'PC' && isPcCourseDtlView ? 'place_course_dtl_view_wrap' : ''}`]">`
    + 
  ```node
  <template>
    <section id="bottom-button-wrap" :class="isPadding ? 'padding-wrap' : ''">
      <button  v-if="single" class="bottom_buttom"  @click.prevent="$emit('click-single')"  :class="isValid ? 'on' : 'off'">
          {{single}}
      </button>
      <div v-else class="bottom_two_buttom" :class="[`${generalBtns ? 'general' : ''}`, `${device === 'PC' && isPcCourseDtlView ? 'place_course_dtl_view_wrap' : ''}`]">
        <button v-if="left" class="left" @click="$emit('click-left')">
          {{left}}
        </button>
        <button v-if="heart" :class=" device === 'PC' ? 'heart heart_course_place':'heart'">
          <input v-model="likeStatus" :value="likeStatus" type="checkbox" @change="handleLikeBtn" id="coursePlaceFavCount">
          <label v-if="isPcCourseDtlView && device === 'PC'" for="coursePlaceFavCount">
            <div class="course_place_fav_count">
              <p>찜 {{coursePlaceFavCount}}</p>
            </div>
          </label>
        </button>
        <button v-if="right" class="right" @click.prevent="$emit('click-right')" :class="isValid ? 'on' : 'off'">
          {{right}}
        </button>
      </div>
    </section>
  </template>

  <style lang="scss" scoped>

  #bottom-button-wrap {
    position: fixed; bottom: 0; right: 0; width: 100%; background: white; z-index: 9999;

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
    .place_course_dtl_view_wrap{
      flex-direction: column-reverse;
      height:94px;
      .right{
        flex: none;
        height: 42px;
        border-radius: 6px;
      }
      .heart_course_place {
        position: relative;
        height: 42px;
        margin-top: 10px;
        width: 100%;
        margin-right: 0;
        border: 1px solid #D1D3D7;
        border-radius: 6px;
        input {

          &::after {
           left: 45%;
          }
        }
        label{
          .course_place_fav_count{
            position: absolute;
            left: 50%;
            top: 29.5%;
            p{
              font-family: roboto;
              font-size: 14px;
              color: #717377;
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
  .padding-wrap {
    padding: 10px 16px; border-top: 1px solid $border;
    button {
      border-radius: 6px; border: 0!important;
    }
    .left {
      border: 1px solid #D1D3D7!important; margin-right: 10px;
    }
  }
    @media (min-width: $minWidth) {
      #bottom-button-wrap {
        position: static; border-radius: 6px!important; overflow: hidden;
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
    props: ['isPadding', 'single', 'isValid', 'left', 'right', 'generalBtns', 'heart', 'likeVal', 'isPcCourseDtlView', 'coursePlaceFavCount'],
    setup() {
      return {

      }
    },
    watch: {
      likeVal(val) {
        this.likeStatus = val
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
