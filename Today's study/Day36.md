#### 리스트에 값이 없을 경우와 있을 경우 페이지 다르게 보이기
+ `<div v-if="handsFreeList.length !== 0" class="top-text">` : `handsFreeList`의 인덱스 길이가 0이 아닐 경우 해당 요소가 보여지게 한다. 즉, 리스트에 값이 있을 경우 보여진다.
+ `<div v-if="handsFreeList.length === 0" class="empty_hands_list_contain">`, `<div v-else class="hands_free_list_contain">`
  + if문을 써서 handsFreeList의 인덱스 길이가 0인 경우 즉, 리스트에 값이 없을 경우 `empty_hands_list_contain` 클래스의 요소를 보여주고 그렇지 않을 경우 `hands_free_list_contain`클래스의 요소를 보여준다.
+ `background="handsFreeList.length > 0 ? '#F5F5F6' : 'white'"` : 리스트에 값이 있으면 `#F5F5F6`이 색, 리스트에 값이 없을 경우 배경색을 흰색으로 바꿔준다.
+ `:header="handsFreeList.length > 0 ? {title: '스마트 핸즈프리', cart: true}: {title: '스마트 핸즈프리', cart: true, back:true} "` : 리스트에 값이 있을 경우에만 헤더를 표출해준다.
```node
<template>
  <ion-page>
      <BaseLayout :header="handsFreeList.length > 0 ? {title: '스마트 핸즈프리', cart: true}: {title: '스마트 핸즈프리', cart: true, back:true} " :useBottomMenu="handsFreeList.length > 0 ? true : false" :background="handsFreeList.length > 0 ? '#F5F5F6' : 'white'">
        <template #con>
          <div class="hands_free_contain">
            <div v-if="handsFreeList.length !== 0" class="top-text">
              <p>스마트핸즈프리소개 문구 이미지명</p>
            </div>
            <div v-if="handsFreeList.length === 0" class="empty_hands_list_contain">
              <div class="empty_hands_list">
                <div class="empty_hands_img">
                  <img src="@/assets/image/temp/purchase.png" alt="hands_empty">
                </div>
                <div class="empty_hands_p">
                  <p>구입 내역이 없습니다.</p>
                </div>
              </div>
            </div>
            <div v-else class="hands_free_list_contain">
              <div v-for="(hands, h) in handsFreeList" :key="h" class="hands_free_list_box">
                <div class="order_num">
                  <span>주문번호&nbsp;20123453123</span>
                </div>
                <div class="hands_free_detail_contain">
                  <div class="hands_free_detail_box">
                    <div class="detail_sec1">
                      <div class="hands_order_date">
                        <span>2021.11.12&nbsp;&nbsp;온라인 주문</span>
                      </div>
                      <div v-if="handsFreeStatus" class="hands_status1">
                        <p>픽업대기</p>
                      </div>
                      <div v-else class="hands_status2">
                        <p>결제대기</p>
                      </div>
                    </div>
                    <div class="detail_sec2">
                      <span>[닥스] 데일리 정장구두[닥스] 데일리 정장구두[닥스] 데일리 정장구두</span>
                    </div>
                    <div class="detail_sec3_box">
                      <div class="detail_sec3">
                        <div class="hands_com_css1">
                          <p>픽업일자</p>
                        </div>
                        <div class="hands_com_css2">
                          <span>2021.08.17&nbsp;오후&nbsp;10:43</span>
                        </div>
                      </div>
                      <div class="detail_sec3">
                        <div class="hands_com_css1">
                          <p>픽업장소</p>
                        </div>
                        <div class="hands_com_css2">
                          <span>대구 국제공항</span>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="go_to_hands_free_detail">
                    <p>상세보기</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </template>
      </BaseLayout>
  </ion-page>
</template>

<script>
import { IonPage } from '@ionic/vue'
export default {
  name: 'HandsFreeList',
  components: {
    IonPage
  },
  data() {
    return {
      handsFreeStatus: true,
      handsFreeList: []
    }
  },
  setup() {
    return {}
  }
}
</script>

<style lang="scss" scoped>
--------------------(생략)----------------------
</style>
```
