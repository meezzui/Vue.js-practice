#### 스와이퍼 변경시 티켓 상태에 따라 배경색 변경
+ 사용전과 사용완료 상태에 따라 티켓의 배경색이 바뀌게 만든다.
+ API 이름
  + `tkIsuSt`: 티켓 상태 (I: 발급, U: 사용, C: 발급취소, E: 만료)
+ `data()`에 `swiper` 오브젝트로 선언해 줌(스와이퍼의 처음 인덱스 값은 0으로 해줌)
  ```node
  swiper: {
          realIndex: 0
            }
  ```
+ `script` 설명
  ```node
  // 스와이퍼 변경 시 티켓 상태에 따라 배경 색 변경
  changeStatus(swiper) { // 선어해준 swiper를 object형태로 받음
    const status = this.ticketInfo[swiper.realIndex].tkIsuSt // 각각의 슬라이드의 상태를 담은 것을 status라는 변수에 넣음. 슬라이드는 각각의 인덱스 값으로 구분.

    this.isTicket = !(status === 'U' || status === 'E') // U: 사용, E: 만료 일때는 배경색이 회색이어야 하는데 isTicket이 false 일때 회색이 되게 css를 해놔서 이렇게 해 줌
  }
  ```
+ `<div v-if="params.prdtGbn === 'T'" class="tourpass_compose" :class="isTicket ? '' : 'off'">`
  + `v-if="params.prdtGbn === 'T'"`: 투어패스일 경우만 보임
  + `:class="isTicket ? '' : 'off'"`: false 일때 배경색 회색
```node
<template>
  <ion-page>
    <BaseLayout :header="{title:title, back:true, backcolor:'blue', displayNone: true}" :fullWidth = "true">
      <template #con>
        <PopupLayout :title="popTitle" :close="close">
          <template #pop-con>
            <div :class="isTicket ? 'ticket-deteil-contain' : 'changed'">
              <div v-if="params.prdtGbn === 'T'" class="tourpass_compose" :class="isTicket ? '' : 'off'">
                <div class="tourpass_p">
                  <p>{{ ticketCommInfo.frcsPrdtName }}</p>
                </div>
                <div class="tourpass_p">
                  <p>{{ ticketCommInfo.optVal }}</p>
                </div>
              </div>
              <div class="ticket-back-box">
                <swiper
                  class="swiper"
                  slidesPerView="1"
                  :modules="modules"
                  :pagination="{  }"
                  @slideChange="changeStatus">
                  <swiper-slide v-for="(ticket, t) in ticketInfo" :key="t">
                    <div class="ticket-backround">
                      <div class="ti-detail-box">
                        <div v-if="params.prdtGbn === 'V'">
                          <span class="ti-detail-title">{{ ticket.frcsNm }}</span>
                        </div>
                        <div class="ti-detail-img" style="position: relative;">
                          <img :src="ticket.tmnlImgUrl" alt="ticket_pro_img"/>
                          <div class="stamp-box-container" v-if="ticket.tkIsuSt === 'U' || ticket.tkIsuSt === 'E'">
                            <div class="stemp-box">
                              <div class="ticket-status-img">
                                <img src="@/assets/image/temp/using-complete-1.png" alt="" class="using-complete-img"/>
                              </div>
                              <div class="ticket-status-container">
                                <div v-if="ticket.tkIsuSt === 'U'" class="ticket-status">
                                  <span>{{ ticket.useDttm }}</span>
                                  <p>사용 완료</p>
                                </div>
                                <div v-if="ticket.tkIsuSt === 'E'" class="ticket-status">
                                  <span>{{ ticketCommInfo.expiDttm }}</span>
                                  <p>기간 만료</p>
                                </div>
                              </div>
                            </div>
                          </div>
                        </div>
                        <div>
                          <p class="ti-detail-contents">{{ ticket.tkNm }}</p>
                        </div>
                        <div v-if="params.prdtGbn === 'V'">
                          <p class="ti-detail-opt">{{ ticket.optVal }}</p>
                        </div>
                        <div class="ti-detail-count-box">
                          <div>
                            <p class="ti-detail-count">수량</p>
                          </div>
                          <div>
                            <span class="ti-detail-count-number">{{ ticket.tkCnt }}</span>
                          </div>
                        </div>
                        <div class="ticket-bottom-box" :class="params.prdtGbn === 'T' ? 'tourTicket-bottom-box' : ''" :style="ticketInfo[0].optVal === null ? 'bottom: -118px' : ''">
                          <div class="ticket-bottom-img1">
                            <img src="@/assets/image/temp/ticketComment.png" alt="" class="comment-img"/>
                          </div>
                          <div class="ticket-bottom-img2">
                            <img src="@/assets/image/temp/stemp.png" alt="" class="stemp-img"/>
                          </div>
                        </div>
                      </div>
                    </div>
                  </swiper-slide>
                </swiper>
              </div>
              <div class="detail-info-box">
                <div class="order-number-box box">
                  <div>
                    <p class="info">주문번호</p>
                  </div>
                  <div>
                    <span class="ti-info number">{{ ticketCommInfo.ordNo }}</span>
                  </div>
                </div>
                <div class="exchange-place-box box" v-if="params.prdtGbn === 'V'">
                  <div>
                    <p class="info">교환처</p>
                  </div>
                  <div>
                    <span class="ti-info place">{{ ticketCommInfo.frcsPrdtName }}</span>
                  </div>
                </div>
                <div class="d-day-box box">
                  <div>
                    <p class="info">유효기간</p>
                  </div>
                  <div>
                    <span class="ti-info period">{{ ticketCommInfo.expiDttm }}</span>
                  </div>
                </div>
              </div>
            </div>
          </template>
        </PopupLayout>
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss">
.ticket-back-box{
  .swiper-pagination {
  position: unset !important;
  margin-top: 20px;
  }
}
</style>
<style lang="scss" scoped>
==============================================(생략)===========================================
.off{
  background-color: #848B93;
}

// 웹 버전 미디어쿼리
@media (min-width: $minWidth) {
  .ticket-deteil-contain{
    height: -webkit-calc(100vh - 192px);
  }
}
</style>

<script>
import { IonPage } from '@ionic/vue'
import { defineComponent } from 'vue'
import { Pagination } from 'swiper'
import { getTicketDetail } from '@/http/modules/myPage'

export default defineComponent({
  name: 'VoucherDetailPage',
  props: ['popTitle', 'close', 'popDetail'],
  components: {
    IonPage
  },
  data() {
    return {
      title: '교환권/티켓',
      isTicket: true,
      ticketList: [1, 1, 1, 1],
      tourpass: true,
      modules: [Pagination],
      params: {},
      dtlParams: {
        ordNo: '',
        ordDtlSeqNo: 0,
        tkNo: 0,
        prdtGbn: ''
      },
      ticketCommInfo: {},
      ticketInfo: [],
      swiper: {
        realIndex: 0
      }
    }
  },
  created() {
    // pc에서 팝업 띄울 때 호출
    this.params = this.popDetail
    this.isTicket = Boolean(this.params.isTicket === 'true')
    this.getDetailInfo()
  },
  ionViewWillEnter() {
    // 모바일에서 페이지 이동 시 호출
    console.log(this.$route.query)
    this.params = this.$route.query
    this.isTicket = Boolean(this.params.isTicket === 'true')
    this.getDetailInfo()
  },
  methods: {
    getDetailInfo() {
      this.dtlParams.ordNo = this.params.ordNo
      this.dtlParams.ordDtlSeqNo = this.params.ordDtlSeqNo
      this.dtlParams.tkNo = this.params.tkNo
      this.dtlParams.prdtGbn = this.params.prdtGbn

      getTicketDetail(this.dtlParams).then(res => {
        this.ticketCommInfo = res.data
        this.ticketInfo = res.data.ticketInfos
        this.changeStatus(this.swiper)
      }).catch(e => {
        console.log(e)
      })
    },
    // 스와이퍼 변경 시 티켓 상태에 따라 배경 색 변경
    changeStatus(swiper) {
      const status = this.ticketInfo[swiper.realIndex].tkIsuSt

      this.isTicket = !(status === 'U' || status === 'E') 
    }
  }
})
</script>
```
