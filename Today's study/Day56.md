#### swiper navigation
+ swiper 모듈에는 `pagination`과 `navigation` 기능이 있다.
+ `pagination`과 `navigation` 모두 페이지를 넘기는 기능이 있는데 `pagination`은 쪽수를 매기는 기능을 하고 `navigation`은 버튼으로 페이지를 넘긴다.

#### swiper navigation 기능 사용하기
+ 먼저 `<swiper @slideChange="setCoursePlaceSwiperIdx">` : 기존 swiper에 `navigation`을 추가해준다.
  + `<swiper @slideChange="setCoursePlaceSwiperIdx" navigation>`
+ 그리고 `SwiperCore`와 `Navigation`을 `import`해준다.  => `import SwiperCore, { Navigation, Pagination } from 'swiper'`
+ 그 다음 아래에 `navigation`을 사용한다는 의미로 `SwiperCore.use([Navigation])`을 선언해준다.
+ 이 위에서 선언해주었기 때문에 `setup()`에서 `return`할 때 `modules`에 `Navigation`은 써주지 않아도 된다❗❗❗
+ 주의✨✨✨
  + `navigation`을 커스텀할 때는 `전역 scss`에다가 해주어야 적용이 된다.
  + `navigation` 위치 조정
    + swpier에서 기본으로 제공하고 있는 css를 보면 `position`으로 되어 있다. 그렇기 때문에 위치를 지정할때 그냥 `left,right`로 조정하면 `navigation`이 이미지 안으로 들어가게 되어 안 보이게 된다.
    + 이를 해결하기 위해서는 상위 태그에 `position="relative"`를 따로 적용시켜 주어야 한다.(이것은 해당 페이지 css에 적용해도 먹힌다.)
  + 이미지 크기에 맞게 자동으로 `navigation`위치 이동
    + 이미지의 높이가 변함에 따라 `navigation`의 위치가 이동하여 위치가 깨지지 않게 해야 한다.
    + `height: max-content`을 사용하면 기존에 지정해둔 `navigation`의 위치가 이미지 높이에 맞게 자동으로 적용된다.
```node
<template>
  <ion-page>
    <BaseLayout id="course_place_detail" :header="{title:'장소 상세', back:true, cart: true}">
      <template #con>
        <div class="place_contain">
          <div class="place_web_left_contain">
            ======================================(생략)================================
            <!--장소 상세 이미지 슬라이드-->
            <section class="course_place_list_sec">
              <swiper @slideChange="setCoursePlaceSwiperIdx" navigation>
                <swiper-slide v-for="(place, p) in dtlImgList" :key="p">
                  <img :src="place" alt="tourpass image">
                </swiper-slide>
              </swiper>
              <p v-if="device === 'MB'">{{coursePlaceSwiperIdx + 1}}/{{dtlImgList.length}}</p>
            </section>
            <!--장소 상세 이미지 슬라이드 끝 -->
            ======================================(생략)==================================
          </div>
          <div class="place_web_right_contain">
            ======================================(생략)==================================
          </div>
        </div>
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss" scoped>
#course_place_detail {
  =============================(생략)==========================
}

// 웹버전 미디어쿼리
@media (min-width: $minWidth){
  #course_place_detail {
    ======================================(생략)================================
}
</style>

<script>
import { IonPage } from '@ionic/vue'
import { ref } from 'vue'
import StarRating from 'vue-star-rating'
import { requestTourPlaceDetail } from '@/http/modules/course'
import { deleteFavorite, registerFavorite, requestReviewList } from '@/http/modules/common'
import SwiperCore, { Navigation, Pagination } from 'swiper'

SwiperCore.use([Navigation])

export default {
  setup() {
    const coursePlaceSwiperIdx = ref(0)
    const setCoursePlaceSwiperIdx = (swiper) => {
      coursePlaceSwiperIdx.value = swiper.realIndex
    }
    return {
      coursePlaceSwiperIdx,
      setCoursePlaceSwiperIdx,
      modules: [Pagination]
    }
  },
  components: {
    IonPage, StarRating
  },
  data() {
    return {
      detailInfo: [],
      dtlImgList: [],
      fvrData: {
        fvrVal: '',
        fvrGbn: ''
      },
      reviewParams: {
        start: 0,
        end: 0,
        size: 0,
        rvGbnVal: '',
        rvGbn: 'S',
        orderGbn: ''
      },
      ifValid: false,
      placeReviewList: [],
      menuTabList: [true, false],
      plNo: 0,
      cpxPrdtNo: '',
      courseList: []
    }
  },
  ionViewWillEnter() {
    this.plNo = this.$route.params.plNo
    this.getPlaceDetail()
    this.getReview()
  },
  methods: {
    clickedMenuTab(idx) {
      for (let i = 0; i < this.menuTabList.length; i++) {
        this.menuTabList[i] = false
      }
      this.menuTabList[idx] = true
    },
    async getPlaceDetail() {
      const res = await requestTourPlaceDetail(this.plNo)
      this.detailInfo = res.data
      this.dtlImgList = this.detailInfo.detailImgs
      this.cpxPrdtNo = this.detailInfo.tourPrdtNo
      this.courseList = this.detailInfo.placeCourseList
    },
    // 찜
    onChangeFavoriteStatus(info, gbn) {
      console.log(gbn)
      if (gbn === 'S') {
        this.fvrData.fvrVal = info.plNo
        this.fvrData.fvrGbn = gbn
      } else {
        this.fvrData.fvrVal = info.cousNo
        this.fvrData.fvrGbn = gbn
      }
      console.log('??', info)
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
          this.getPlaceDetail()
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
        plNo: this.plNo,
        fvrYn: bool
      }
      this.onChangeFavoriteStatus(fvrData, 'S')
    },
    // 리뷰 등록 페이지로 이동
    goReview() {
      this.$router.push({ name: 'ReviewWrite', params: { id: this.plNo, rv: 'S', ordNo: 0 }})
    },
    // 등록된 리뷰 조회
    async getReview() {
      this.reviewParams.rvGbnVal = this.plNo
      const res = await requestReviewList(this.reviewParams)
      this.placeReviewList = res.data.reviews
    },
    // 코스 상세 이동
    goCourseDetail(cousNo) {
      this.$router.push({ name: 'courseDetail', params: { cousNo: cousNo }})
    },
    // 연관 상품 둘러보기 클릭
    goTourList(info) {
      this.$router.push({ name: 'TourPassList', params: { dlNo: info.tourPrdtNo }})
    }
    // TODO 도보 네비게이션
  }
}
</script>
```
