#### props로 button custom해서 넘기기
+ `commponent` 파일에 공통으로 빼둔 `BottomButton.vue` 에서 `props`를 통해 custom한 버튼을 받아서 사용!!
+ 먼저 공통으로 빼둔 `BottomButton.vue`의 코드를 봐보자(자식 컴포넌트)😃
  + 버튼은 single버튼과 two_button이 있다.
  + `<section id="bottom-button-wrap" :class="isPadding ? 'padding-wrap' : ''">`: 버튼에 padding 주기 => 사용 페이지에서 `isPadding`으로 버튼에 padding 유무를 정할 수 있다.
  + `<button  v-if="single" class="bottom_buttom"  @click.prevent="$emit('click-single')"  :class="isValid ? 'on' : 'off'">`
    + `v-if="single"`: single일 때만 보임
    +  `@click.prevent="$emit('click-single')"`: 버튼 클릭 이벤트 => 사용 페이지에서 `click-single="함수이름()"`이런 형식으로 사용하여 해당 버튼을 클릭 시 해당 함수가 실행되게 할 수 있다.
    + `:class="isValid ? 'on' : 'off'"`: `isValid`가 true이면 .on 클래스의 css가 적용, false이면 .off 클래스의 css가 적용된다. 이것은 체크박스에 체크가 되거나/input태그에 입력값이 입력되어야 버튼이 활성화되게 할 때 사용페이지에서 바인딩하여 사용한다. => `:isValid=""`
  + `{{single}}`: single버튼이 props를 통해 보이게 하기 위해 `{{}}`이걸로 묶어서 나타냄
  + `<div v-else class="bottom_two_buttom" :class="[`${generalBtns ? 'general' : ''}`, `${device === 'PC' && isPcCourseDtlView ? 'place_course_dtl_view_wrap' : ''}`]">`
    + `v-else`: 버튼 두개짜리가 보여짐
    + `:class="[`${generalBtns ? 'general' : ''}`, `${device === 'PC' && isPcCourseDtlView ? 'place_course_dtl_view_wrap' : ''}`]"`: 클래스를 배열로 묶어 해당 조건에 맞는 css가 적용됨
  + `<button v-if="left" class="left" @click="$emit('click-left')">`: 버튼 두개 중 왼쪽 버튼을 나타냄
  + `<button v-if="heart" :class=" device === 'PC' ? 'heart heart_course_place':'heart'">`: 왼쪽 버튼에 하트를 나타내고 싶은 경우 사용 페이지에서 `heart="true"`이렇게 해주면 됨
  + `:class=" device === 'PC' ? 'heart heart_course_place':'heart'"`: PC버전과 Moblie버전의 버튼 모양과 위치가 다르기 때문에 device에 따라 css를 다르게 적용해주었다.
  + `<input v-model="likeStatus" :value="likeStatus" type="checkbox" @change="handleLikeBtn" id="coursePlaceFavCount">`
    + input태그의 type을 체크박스로 하고 아래 css에서 background로 이미지를 하트 이미지로 하면 우리가 흔히 아는 찜 버튼이 만들어진다.
    + `v-model="likeStatus"`: data에 `likeStatus`을 false로 선언해준다. 그리고 `likeVal(val) { this.likeStatus = val }`로 체크박스를 클릭할 때마다 true/false로 값이 바뀌게 한다.
    + `@change="handleLikeBtn"`: 체크박스의 상태를 자식 컴포넌트에서 부모 컴포넌트에 데이터를 전달한다.
  + `<label v-if="isPcCourseDtlView && device === 'PC'" for="coursePlaceFavCount">`
    + `v-if="isPcCourseDtlView && device === 'PC'"`: `isPcCourseDtlView`가 true일 때랑 PC버전일 때만 이 label태그가 보인다. => 이 label태그는 찜의 갯수를 나타낸다.
  + `<p>찜 {{coursePlaceFavCount}}</p>`: 찜 개수 
  + `<button v-if="right" class="right" @click.prevent="$emit('click-right')" :class="isValid ? 'on' : 'off'">`
    + `v-if="right"`: 오른쪽 버튼이 보임
    + `:class="isValid ? 'on' : 'off'"`: 버튼 활성화 관련 css

  + `props: ['isPadding', 'single', 'isValid', 'left', 'right', 'generalBtns', 'heart', 'likeVal', 'isPcCourseDtlView', 'coursePlaceFavCount'`
    + 위에 것들을 props로 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달한다.
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
        this.$emit('like', this.likeStatus)  // 자식 컴포넌트에서 부모 컴포넌트에 데이터 전달
      }
    }
  })
  </script>
  ```
+ 사용 페이지(부모 컴포넌트)😊
  + `:isPadding="device === 'MB'"`: 모바일 버전에만 버튼에 패딩이 적용되게 한다.
  + `:heart="true"`: 찜 버튼 보이기(하트 이미지)
  + `right="연관 투어상품 둘러보기"`: 오른쪽 버튼 보이기. 버튼에 보여지는 문구는 `연관 투어상품 둘러보기`이다.
  + `:isValid="courseInfo.tourCpxPrdtLinkYn === 'Y'"`: 관련된 코스가 있을 경우에만 오른쪽 버튼이 활성화 됨
  + `isPcCourseDtlView="true"`: PC버전 버튼 보여짐(모바일 버전과는 보이는 버튼의 정렬과 순서가 다름)
  + `:coursePlaceFavCount="courseInfo.fvrCnt"`: 찜 개수
  + `:likeVal="courseInfo.fvrYn"`: 찜 상태 여부
  + `@like="setFvrData"`: 하단 찜 여부 
  + `@click-right="goTourList(courseInfo)"`: 오른쪽 버튼을 누르면 해당 관련 코스 상세 페이지로 
  ```node
  ================================(생략)==================================

  <BottomButton :isPadding="device === 'MB'" :heart="true" right="연관 투어상품 둘러보기" :isValid="courseInfo.tourCpxPrdtLinkYn === 'Y'" isPcCourseDtlView="true"        :coursePlaceFavCount="courseInfo.fvrCnt" :likeVal="courseInfo.fvrYn" @like="setFvrData" @click-right="goTourList(courseInfo)"/>

  ==================================(생략)===============================
  <script>
  import { IonPage } from '@ionic/vue'
  import { ref } from 'vue'
  import StarRating from 'vue-star-rating'
  import { requestTourCourseDetail } from '@/http/modules/course'
  import { deleteFavorite, registerFavorite, requestReviewList } from '@/http/modules/common'
  import SwiperCore, { Navigation, Pagination } from 'swiper'

  SwiperCore.use([Navigation])

  export default {
    setup() {
      const courseSwiperIdx = ref(0)
      const setCourseSwiperIdx = (swiper) => {
        courseSwiperIdx.value = swiper.realIndex
      }
      return {
        courseSwiperIdx,
        setCourseSwiperIdx,
        modules: [Pagination]
      }
    },
    components: {
      IonPage, StarRating
    },
    data() {
      return {
        courseNo: 0,
        courseInfo: [],
        courseDtlImgs: [],
        coursePlan: [],
        fvrData: {
          fvrVal: '',
          fvrGbn: ''
        },
        reviewParams: {
          start: 0,
          end: 0,
          size: 0,
          rvGbnVal: '',
          rvGbn: 'C',
          orderGbn: ''
        },
        courseReviewList: [],
        courseList: [1, 1, 1],
        courseMenuTabList: [true, false],
        coursePlanList: [1, 1, 1, 1, 1, 1]
      }
    },
    created() {
      this.courseNo = this.$route.params.cousNo
      this.getCourseInfo()
      this.getReview()
    },
    ionViewWillEnter() {
      this.courseNo = this.$route.params.cousNo
      this.getCourseInfo()
      this.getReview()
    },
    methods: {
      clickedMenuTab(idx) {
        for (let i = 0; i < this.courseMenuTabList.length; i++) {
          this.courseMenuTabList[i] = false
        }
        this.courseMenuTabList[idx] = true
        console.log(this.courseMenuTabList)
      },
      // 코스 상세 정보 조회
      async getCourseInfo() {
        const res = await requestTourCourseDetail(this.courseNo)
        this.courseInfo = res.data
        this.courseDtlImgs = this.courseInfo.detailImgs
        this.coursePlan = this.courseInfo.schedules
        console.log(this.courseInfo)
      },
      // 찜
      onChangeFavoriteStatus(info, gbn) {
        this.fvrData.fvrVal = info.cousNo
        this.fvrData.fvrGbn = gbn
        if (info.fvrYn) {
          registerFavorite(this.fvrData).then(res => {
          }).catch(err => {
            if (err.status === 401) {
              console.log(err)
            } else {
              this.$alert({
                msg: err.errorMessage,
                success: () => {
                  info.fvrYn = false
                  err.success ? this.$router.push({ name: err.success }) : ''
                }
              })
            }
          }).finally(_ => {
            this.getCourseInfo()
          })
        } else {
          deleteFavorite(this.fvrData).then(res => {

          }).catch(err => {
            if (err.status === 401) {
              console.log(err)
            } else {
              this.$alert({
                msg: err.errorMessage,
                success: () => {
                  info.fvrYn = false
                  err.success ? this.$router.push({ name: err.success }) : ''
                }
              })
            }
          }).finally(_ => {

          })
        }
      },
      // 하단 찜 버튼 데이터 세팅
      setFvrData(bool) {
        const fvrData = {
          cousNo: this.courseNo,
          fvrYn: bool
        }
        this.onChangeFavoriteStatus(fvrData, 'C')
      },
      // 연관 상품 둘러보기 클릭
      goTourList(info) {
        this.$router.push({ name: 'TourPassList', params: { cpxPrdtNo: info.tourPrdtNo }})
      },
      // 등록된 리뷰 조회
      async getReview() {
        this.reviewParams.rvGbnVal = this.courseNo
        const res = await requestReviewList(this.reviewParams)
        this.courseReviewList = res.data.reviews
      },
      // 리뷰 등록 페이지로 이동
      goReview() {
        this.$router.push({ name: 'ReviewWrite', params: { id: this.courseNo, rv: 'C', ordNo: 0 }})
      },
      // 장소 상세 이동
      goPlaceDetail(plNo) {
        this.$router.push({ name: 'placeDetail', params: { plNo: plNo }})
      }
    }
  }
  </script>
  ```










