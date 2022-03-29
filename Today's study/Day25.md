#### 선택한 것만 디테일 펼쳐지기
+ 펼치기 이미지를 클릭하면 상세페이지가 펼쳐지는데 클릭한 것만 펼쳐진다.
+ checked 속성을 이용한다.
+ `<div v-for="(written, i) in writtenList" :key="i" class="written-review-list-box">`
  + `writtenList`에 배열로 object들을 넣어주준다. -> ` writtenList: [{ rating: 3 }, { rating: 3 }, { rating: 3 }, { rating: 3 }, { rating: 3 }]`
+ `<div class="written-review-contents" @click="written.checked = !written.checked">`
  + `@click="written.checked = !written.checked"` : check값을 반대로 바꿔주는 역할을 한다.(true면 열리고 false면 닫히고)
+ `<img v-if="!written.checked" src="@/assets/image/temp/down-arrow.png" alt="" @click="clickedShowBtn()">`
  + `v-if="!written.checked"`: check가 ture이면 이 이미지가 보인다.
  + `@click="clickedShowBtn()"`: 이미지가 클릭되었을 경우 written 값을 반대로 바꿔주는 역할
```node
<template>
  <ion-page>
    <BaseLayout :header="{title:title, back:true}">
      <template #con>
        <div class="pro-review-contain">
          <div class="pro-review-header">
            <div class="ava-review">
                <button :class="isReview ? 'ava-review-tab on' : 'off'" @click="clickedButton(true)">작성 가능한 리뷰</button>
            </div>
            <div class="written-review">
                <button :class="!isReview ? 'written-review written':'off'"  @click="clickedButton(false)">작성한 리뷰</button>
            </div>
          </div>
          <template v-if="isReview">
            <div class="ava-review-list-contain">
              <div v-for="(review, i) in reviewList" :key="i" class="ava-review-list-box">
                <div class="review-date">
                  <span>2022.11.30&nbsp;(20223453123)</span>
                </div>
                <div class="ava-review-contents">
                  <div class="pro-contents">
                    <div class="pro-img">
                      <img src="@/assets/image/temp/cancel-shoes.png" alt="">
                    </div>
                    <div class="pro-detail-box">
                      <div class="pro-name">
                        <span>[닥스] 데일리 정장구두</span>
                      </div>
                      <div class="pro-detail">
                        <span>블랙,&nbsp;235</span>
                      </div>
                    </div>
                  </div>
                  <div class="go-to-write">
                    <img src="@/assets/image/temp/review-icon.png" alt="">
                  </div>
                </div>
              </div>
            </div>
          </template>
          <template v-else>
            <div class="written-review-list-contain">
              <div v-for="(written, i) in writtenList" :key="i" class="written-review-list-box">
                <div class="written-review-contents" @click="written.checked = !written.checked">
                  <div class="pro-contents">
                    <div class="pro-img">
                      <img src="@/assets/image/temp/cancel-shoes.png" alt="">
                    </div>
                    <div class="written-pro-detail-box">
                      <div class="written-pro-name">
                        <span>[닥스] 데일리 정장구두</span>
                      </div>
                      <div class="written-pro-detail">
                        <span>블랙,&nbsp;235</span>
                      </div>
                    </div>
                  </div>
                  <div class="review-detail">
                    <img v-if="!written.checked" src="@/assets/image/temp/down-arrow.png" alt="" @click="clickedShowBtn()">
                    <img v-else src="@/assets/image/temp/up-arrow.png" @click="clickedShowBtn()">
                  </div>
                </div>
                 <!--상세 리뷰란 시작-->
                <div v-if="written.checked" class="review-box">
                  <div class="like-star">
                    <star-rating
                      :rating="written.rating"
                      :read-only="true"
                      class="star-rating-component"
                      :increment="1"
                      text-class="rating-number"
                      :border-width="2"
                      :star-size="10"
                      :show-rating="false"
                      border-color="#D1D3D7"
                      active-color="#FABB51"
                      :star-points="[23,2, 14,17, 0,19, 10,34, 7,50, 23,43, 38,50, 36,34, 46,19, 31,17]"
                      inactive-color="#D1D3D7"
                      active-border-color="#FABB51">
                    </star-rating>
                  </div>
                  <div class="review-coment">
                    <p>떡볶이에서 이런 맛을 느낄 수 있다고? 로제 떡볶이가 짱이에요! 떡볶이에서 이런 맛을 느낄 수 있다고</p>
                  </div>
                  <div class="review-write-date">
                    <span>2021.12.31</span>
                  </div>
                </div>
                 <!--상세 리뷰란 끝-->
              </div>
            </div>
          </template>
        </div>
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss" scoped>
--------------(생략)---------------------
</style>

<script>
import { IonPage } from '@ionic/vue'
import { defineComponent } from 'vue'
import StarRating from 'vue-star-rating'

export default defineComponent({
  name: 'AvailableReviewPage',
  components: {
    IonPage, StarRating
  },
  data() {
    return {
      title: '상품 리뷰',
      isReview: true,
      reviewList: [1, 1, 1, 1, 1, 1],
      writtenList: [{ rating: 3 }, { rating: 3 }, { rating: 3 }, { rating: 3 }, { rating: 3 }],
      showReview: true
    }
  },
  created() {
  },
  ionViewWillEnter() { // 페이지에 들어가서, 페이지가 활성활 되었을때 적용(ionic에서만 가능). 선언해준 'getList()'를 사용하기 위해 여기에 넣어서 선언해줌(생명주기)
    this.getList()
  },
  methods: {
    getList() {
      this.writtenList.forEach((r, i) => { //r은 writtenList의 i번째를 가리킨다.
        r.checked = false
      })
    },
    clickedButton(val) {
      this.isReview = val
    },
    clickedShowBtn() {
      this.showReview = !this.showReview
    }
  }
})
</script>

