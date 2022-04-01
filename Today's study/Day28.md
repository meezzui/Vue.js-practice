### 메뉴 탭 구현방법
+ 메뉴 탭을 구현하는 방법은 두 가지이다.
+ `data에 변수를 선언하고 boolean값을 주고 그것을 method:{ } 에서 value값이 바뀌게 하는 방법` : 2개짜리 탭은 가능하지만 3개부터는 이것을 이용할 수 없다.
+ `data에 변수를 배열로 선언하여 boolean값을 주고 method:{ } 에서 배열의 index 번호를 사용하는 방법` : 이것은 탭의 갯수 상관없이 쓸 수 있다.(배열에 값만 더 추가해주면 됨)

#### value값을 이용하여 탭 구현
```node
<div class="title-event">
  <button :class="isEvent ? 'event-tab on' : 'off'" @click="clickedButton(true)">이벤트</button>
</div>
<div class="title-notice">
  <button :class="!isEvent ? 'notice-tab on':'off'"  @click="clickedButton(false)">공지사항</button>
</div>

 
<style lang="scss" scoped>
.on{border-bottom: 1.5px solid #22BDB6; color: #22BDB6; font-size: 14px;} // border-bottom 으로 언더바 생성
.event-tab, .notice-tab{height: 30px;} //header라인이랑 맞게 button태그 height를 줌
</style>
  
<script>
data() {
  return {
    isEvent: true
  }
},
methods: {
  clickedButton(val) {
    this.isEvent = val
  }
}
</script>
```
+ `isEvent`를 `true`로 초기값으로 잡는다.
+ 이벤트 버튼의 클릭함수를 `true`로 놓는다. -> 버튼이 이미 눌린 상태로 보여짐( `#22BDB6` 이 색깔로 보여짐)
+ `clickedButton(val) { this.isEvent = val }` : 클릭했을 때 작동하는 함수 -> true/false를 넣어줌
+ `:class="isEvent ? 'event-tab on' : 'off'"` : 클래스 바인딩 -> `isEvent`가 true이면 클래스 `on`이 작동하고 false이면 클래스 `off`가 작동한다.
+ 여기서 중요한 점!!!🧨 공지사항은 반대로 작용하니까 클래스 바인딩할 때 `!isEvent` 이렇게 써줘야 한다.
  
#### 배열의 인덱스값을 이용해서 탭 구현하기
+ `data()`에 ` tabList: [true, false, false]`를 선언해준다. true로 되어있는 것이 처음에 보이는 탭이다.
+ 그 다음 `methods:` 쪽에 js를 작성해준다. 클릭 이벤트 함수의 이름은 `clickedTab`이다.
```node
methods: {
  clickedTab(idx) { //클릭 이벤트 함수가 실행되었을때 idx값을 넘겨준다.
    for (let i = 0; i < this.tabList.length; i++) { // tabList의 배열길이 만큼 돌면서
      this.tabList[i] = false // 그 배열자리에 boolean값을 fasle로 바꿔준다.
    }

    this.tabList[idx] = true // 그리고 다시 true로 바꿔준다.
    console.log(this.tabList)
  }
```
+ `<div :class="tabList[0] ? 'on': 'off'" @click="clickedTab(0)">`
  + 해당 탭 부분에 클래스 바인딩을 하여 삼항연산자를 써준다.
  + `tabList[0]`: 리스트의 첫번째 요소를 의미하며 true일때는 'on'클래스가 'false'일 때는 'off'클래스가 작동한다.
  + `@click="clickedTab(0)"`: 클릭 이벤트를 사용하여 `clickedTab(0)` 이 함수의 0번째 인덱스를 넘겨준다.
+ `<template v-if="tabList[0]">`: 리스트의 0번째의 탬플릿 태그 요소의 것들이 보인다.(탭 메뉴를 누르면 해당 요소들만 보이게 하기위해 메뉴마다 템플릿을 따로 주었다.)
```node
<template>
  <ion-page>
    <BaseLayout id="tour_pass_detail" :header="{title:'투어패스 상세', back:true, cart: true}">
      <template #con>
        <div class="tourpass-contain">
          <!--메뉴 텝 시작-->
          <div class="tab-layer-contain">
            <div class="tab-header-box">
              <div :class="tabList[0] ? 'on': 'off'" @click="clickedTab(0)">
                <p>상품정보</p>
              </div>
              <div :class="tabList[1] ? 'on': 'off'" @click="clickedTab(1)">
                <p>상세정보</p>
              </div>
              <div :class="tabList[2] ? 'on': 'off'" @click="clickedTab(2)">
                <div>
                  <p>리뷰 <span class="review-count">999+</span></p>
                </div>
              </div>
            </div>
            <template v-if="tabList[0]">
              <!--상품정보란-->
              <div v-html="productDetail" class="tour-info-pro-contain"></div>
            </template>
            <template v-else-if="tabList[1]">
              <!--상세정보란-->
              <div class="tour-detail-info-box">
                <div class="tour-info">
                  <div>
                    <p>유효기간</p>
                  </div>
                  <div>
                    <span>2022-02-15</span>
                  </div>
                </div>
                <div class="tour-info">
                  <div>
                    <p>티켓 사용 방법</p>
                  </div>
                  <div>
                    <span>티켓 사용 방법 설명입니다. 티켓 사용 방법설명입니다. 티켓 사용 방법 설명입니다.티켓 사용 방법 설명입니다. 티켓 사용 방법 설명입니다. 티켓 사용 방법 설명입니다. 티켓 사용 방법 설명입니다.</span>
                  </div>
                </div>
                <div class="tour-info">
                  <div>
                    <p>유의사항</p>
                  </div>
                  <div>
                    <span>
                      · 유의사항 안내입니다. 유의사항안내입니다. 유의사항
                        안내입니다.<br>
                      · 유의사항 안내입니다. 유의사항안내입니다. 유의사항
                        안내입니다.<br>
                      · 유의사항 안내입니다.<br>
                      · 유의사항 안내입니다.
                    </span>
                  </div>
                </div>
                <div class="tour-info">
                  <div>
                    <p>취소/환불 안내</p>
                  </div>
                  <div>
                    <span>
                      취소/환불 안내 등의 정보 노출됩니다.<br>
                      취소/환불 안내 등의 정보 노출됩니다.<br>
                      취소/환불 안내 등의 정보 노출됩니다.<br>
                      취소/환불 안내 등의 정보 노출됩니다.
                    </span>
                  </div>
                </div>
              </div>
            </template>
            <template v-else>
              <!--리뷰 란-->
              <div class="tour-review-contain">
                <div class="write-review-box">
                  <div class="star-box">
                    <div>
                      <img src="@/assets/image/temp/Rating.png" alt="rasing-star">
                    </div>
                    <div>
                      <p>4.2</p>
                    </div>
                  </div>
                  <div class="write-box">
                    <div>
                      <img src="@/assets/image/temp/review-write.png" alt="review-write">
                    </div>
                    <div>
                      <p>리뷰쓰기</p>
                    </div>
                  </div>
                </div>
                <div class="review-view-contain">
                  <div v-for="(reviews, i) in tourReviewList" :key="i" class="review-view-box">
                    <div class="star-rating-box">
                      <star-rating
                        :rating="3"
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
                    <div class="review-text">
                      <p>너무 맛있어요.<br>인생 떡볶이 입니다.</p>
                    </div>
                    <div class="review-date-box">
                      <div>
                        <span>2021.12.31</span>
                      </div>
                      <div>
                        <span>extf*****</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </template>
          </div>
          <!--메뉴 텝 끝-->
        </div>
        <div>
          <BottomButton heart=true right="구매" isValid="false" isPadding= "true" @click-right="clickedPurchase()"/>
        </div>
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss" scoped>
----------------(생략)------------------
</style>

<script>
import { IonPage } from '@ionic/vue'
import { ref } from 'vue'
import StarRating from 'vue-star-rating'

export default {
  components: {
    IonPage, StarRating
  },
  data() {
    return {
      tourpassList: [1, 1, 1],
      tabList: [true, false, false],
      tourReviewList: [1, 1, 1, 1, 1],
      productDetail: '상품정보 test!!'
    }
  },
  created() {
  },
  methods: {
    clickedTab(idx) {
      for (let i = 0; i < this.tabList.length; i++) {
        this.tabList[i] = false
      }

      this.tabList[idx] = true
      console.log(this.tabList)
    },
    clickedPurchase() {
      this.$alert({
        title: '장바구니에 상품이 담겼습니다.',
        left: '계속 쇼핑',
        right: '장바구니 이동',
        img: 'bascket'
      })
    }
  }
}
</script>

```
