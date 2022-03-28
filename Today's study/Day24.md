#### 목록 접었다 펼쳤다(접혀있을 땐 '▼' 이미지 펼쳐졌을 땐 '▲' 이미지)
+ 목록에 화살표 방향을 클릭하면 주문 상세 내역을 볼 수 있으며 화살표 방향이 바뀐다. 그 화살표를 클릭하면 다시 목록이 접힌다.
+ 먼저 `data`에 `orderDetailDown: true`을 선언해준다. 그리고 img태그에 클릭 이벤트를 선언해준다. `@click="clickedDownBtn1()"`
+ `script`안에 `clickedDownBtn1()` 이 함수는 클릭되었을 때 `this.orderDetailDown = !this.orderDetailDown`를 실행하게 되는데 기존에 `orderDetailDown`값을 `false`로 바꿔준다.
+ `<img v-if="!orderDetailDown" src="@/assets/image/temp/down-arrow.png" @click="clickedDownBtn1()">` : `v-if="!orderDetailDown"`으로 인해 이미지가 클릭되었을 때 `orderDetailDown`이 false인 경우 '▼'이미지가 보인다.
+ `<img v-else src="@/assets/image/temp/up-arrow.png" @click="clickedDownBtn1()">` : `v-else`로 인해 이미지가 클릭되면 '▲'이미지로 바뀐다.
+ `<div v-if="orderDetailDown" class="or-detail-contents-box">`: `orderDetailDown`이 true일 경우에만 해당 div안에 요소들이 보인다.
```node
<template>
  <ion-page>
    <BaseLayout :header="{title:title, back:true}">
      <template #con>
        <div class="or-contain">
          <div class="or-detail-layer1-box">
            <div>
              <span>2021.11.30</span>
            </div>
            <div>
              <p>주문번호&nbsp;<span>20123453123</span></p>
            </div>
          </div>
          <div class="or-detail-middle-line"></div>
          <div class="or-detail-layer2-box">
            <div class="or-detail-layer2-inner1">
              <div class="or-detail-contents-header-box">
                <div>
                  <p>주문 상품 정보</p>
                </div>
                <div>
                  <img v-if="!orderDetailDown" src="@/assets/image/temp/down-arrow.png" @click="clickedDownBtn1()">
                  <img v-else src="@/assets/image/temp/up-arrow.png" @click="clickedDownBtn1()">
                </div>
              </div>
              <!-- 주문 상세 정보란 시작-->
              <div v-if="orderDetailDown" class="or-detail-contents-box">
                <div v-for="(order, i) in orderList" :key="i" class="or-detail-contents">
                  <div class="brand-title">
                    <div>
                      <span>신참떡볶이</span>
                    </div>
                    <div>
                      <img src="@/assets/image/temp/telephone.png">
                    </div>
                  </div>
                  <div class="detail-box">
                    <div class="or-brand-img">
                      <img src="@/assets/image/temp/order-brand.png">
                    </div>
                    <div class="or-brand-detail">
                      <div>
                        <span>떡볶이+튀김+순대</span>
                      </div>
                      <div>
                        <div class="discount-price">
                          <span>17,800원</span>
                        </div>
                        <div class="origin-price">
                          <span>25,000원</span>
                        </div>
                      </div>
                      <div class="or-common-css">
                        <p>상태:&nbsp;<span :class="statusType ? '' : 'off'">결제완료</span></p>
                      </div>
                      <div class="or-common-css">
                        <p v-if="howToBuy">구매:&nbsp;<span>현장교환</span></p>
                        <div v-else class="buy-to-handsFree">
                          <div class="diff-type">
                            <p>구매:</p>
                          </div>
                          <div class="diff-buy-type">
                            <div class="hands-free-img">
                              <img src="@/assets/image/temp/handsFree.png" alt="">
                            </div>
                            <div class="buy-hands-free">
                              <p>핸즈프리</p>
                            </div>
                          </div>
                        </div>
                      </div>
                      <div class="or-common-css">
                        <p>수량:&nbsp;<span>1개</span></p>
                      </div>
                    </div>
                  </div>
                  <div class="choice-box">
                    <div v-if="btnType" class="btn-type1">
                      <div @click="routeTo('requestCancel')">
                        <p>결제 취소</p>
                      </div>
                      <div @click="routeTo('voucherDetail')">
                        <p>교환권 상세</p>
                      </div>
                    </div>
                    <div v-else class="btn-type2" @click="routeTo('cancelDetaill')">
                      <div>
                        <p>취소 정보</p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <!-- 주문 상세 정보란 끝-->
            </div>
            <div class="or-detail-layer2-inner2">
              <div class="or-pay-contents-header-box">
                <div>
                  <p>결제 정보</p>
                </div>
                <div>
                  <img v-if="!payDetailDown" src="@/assets/image/temp/down-arrow.png" @click="clickedDownBtn2()">
                  <img v-else src="@/assets/image/temp/up-arrow.png" @click="clickedDownBtn2()">
                </div>
              </div>
            -----------------------------------------(생략) ----------------------------------------------------
        </div>
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss" scoped>
------------------(생략)--------------
</style>

<script>
import { IonPage } from '@ionic/vue'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'OrderDetailPage',
  components: {
    IonPage
  },
  data() {
    return {
      title: '구매 내역 상세',
      btnType: true,
      orderList: [1, 1, 1, 1],
      orderDetailDown: true,
      payDetailDown: true,
      statusType: true,
      howToBuy: false
    }
  },
  created() {
  },
  methods: {
    clickedDownLoad() {
      this.isDownLoad = true
    },
    clickedDownBtn1() {
      this.orderDetailDown = !this.orderDetailDown
    },
    clickedDownBtn2() {
      this.payDetailDown = !this.payDetailDown
    }
  }
})
</script>
```
