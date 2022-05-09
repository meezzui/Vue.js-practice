#### PC버전일 경우 모달창 띄우기(API 연동)
+ 모바일 버전에서는 클릭 시 해당 페이지로 이동하는 형태이지만 웹페이지 버전에서는 해당 페이지가 팝업으로 띄어지는 방식으로 구현
+ 웹버전 모달창 띄우기
+ 🧨리스트 페이지와 상세페이지🧨
+ 리스트 페이지(교환권/티켓)✨ => 교환권을 클릭하면 해당 디테일 페이지 팝업창이 뜬다.
  + 교환권 contain div태그에 `@click="device === 'PC' ? 'openModal('ticket')' : 'goTicketDetail(ticket)'"` 이벤트를 걸어준다.
  + `@click="device === 'PC' ? 'openModal('ticket')' : 'goTicketDetail(ticket)'"`: `device`가 `'PC'`일 경우 `openModal()`을 타서 팝업창이 열리고 모바일 버전일 경우 `goTicketDetail()`함수를 타게 되어 페이지가 이동을 하게 된다.
  + `openModal()` 코드 설명
  ```node
  // pc에서 모달창 띄우기
    async openModal(flag) { //flag를 통해 object 형태로 받음
      const components = {
        ticket: { title: '교환권/티켓', comp: ticketDetail, class: 'ticket-detail-h' } // 팝업창에 '교환권/티켓'이라는 제목 부분과 팝업창 크기를 지정(`ticket-detail-h` 전역 파일에 이 클래스 이름으로 지정)
      const modal = await modalController // `modalController` 라는 ionic의 기능 사용
        .create({
           component: components[flag].comp, // object형태로 ticketDetail이라는 이름으로 사용 => ticketDetail은 디테일페이지 import 이름 `import ticketDetail from './VoucherDetail.vue'`
           cssClass: ['pc-modal-sm', components[flag].class], // 팝업창의 width(`pc-modal-sm` 클래스 이름)랑 height 지정(전역 css에 지정)
          componentProps: { // 아래것들을 상세페이지에 props로 넘겨줌
            popTitle: components[flag].title,
            close: () => { // X 이미지 클릭 시 팝업 
              modal.dismiss()
            }
          }
        })
      return modal.present()
    }
  }
  ```
```node
<template>
  <ion-page>
    <BaseLayout :header="{title:title, back:true,  disableScrollEvent: true}"
                :background="(device === 'MB' && ((isTicket && avlbTickets.length > 0) || !isTicket && unavlbTickets.length > 0)) || device === 'PC' ? '#F5F5F6' : 'white'">
      <template #con>
        <MyLayout title="교환권/티켓">
          <template #my-layout-con>
====================================================================(생략)============================================================================
            <div v-if="(isTicket && avlbTickets.length === 0) || (!isTicket && unavlbTickets.length === 0)" class="empty_ticket_list_contain">
              <div class="empty_ticket_list">
                <div class="empty_ticket_img">
                  <img src="@/assets/image/temp/exchange-ticket.png" alt="ticket_empty">
                </div>
                <div class="empty_ticket_p">
                  <p>교환권/티켓이 없습니다.</p>
                </div>
              </div>
            </div>
            <div v-else class="ticket-contain">
              <div class="ticket-contents-box box-margin" role="btn" v-for="(ticket,t) in (isTicket ? avlbTickets : unavlbTickets)" :key="t" :class="isTicket ? '' : 'expire'" @click="device === 'PC' ? 'openModal('ticket')' : 'goTicketDetail(ticket)'">
                <div class="ticket-contents">
                  <div class="ticket-contents-img">
                    <img :src="ticket.tmnlImgUrl" alt="" class="contents-img"/>
                  </div>
                  <div class="ticket-detail-box" :class="ticket.optVal === null || ticket.frcsNm === null ? 'option-none' : ''">
                    <div class="ticket-brand" v-if="ticket.frcsNm !== null">
                      <span class="ti-brand">{{ ticket.frcsNm }}</span>
                    </div>
                    <div class="ticket-title">
                      <span class="ti-title">{{ ticket.prdtNm }}</span>
                    </div>
                    <div class="ticket-option" v-show="ticket.optVal !== null">
                      <span class="ti-option">{{ ticket.optVal }}</span>
                    </div>
                    <div class="ticket-date-box" :class="ticket.optVal !== null && ticket.frcsNm !== null ? 'option-exist' : ''">
                      <div v-if="isTicket" class="ticket-remaining-period">
                        <span class="ti-remain">{{ ticket.leftDays }}일 남음</span>
                      </div>
                      <div class="ticket-period">
                        <span class="ti-period">{{ ticket.expiDttm }} 까지</span>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="using-ticket">
                  <p v-if="isTicket" class="using">사용하기</p>
                  <template v-else>
                    <p v-if="ticket.tkSt === 'U'" class="using-complete">사용완료</p>
                    <p v-if="ticket.tkSt === 'E'" class="period-complete">기간만료</p>
                  </template>
                </div>
              </div>
            </div>
          </template>
        </MyLayout>
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss" scoped>
=======================================(생략)================================================
//웹 버전 미디어 쿼리
@media (min-width: $minWidth) {
  .on{
    color: black; border-radius: 6px; padding-bottom: 0; width: 100%; height: 100%;
    background-color: white; border: 1px solid #717377;
    }
    .off{
      padding: 15px 0;
    }
  .ticket-header{
    position: static;
    width: 255px;
    height: 46px;
    margin: 0 auto;
    border: 1px solid #D1D3D7;
    border-radius: 6px;
    background-color: #F5F5F6;
    .ticket-unuse{
      height: 100%;
    }
    .ticket-use{
      height: 100%;
    }
  }
}
</style>

<script>
import { IonPage, modalController } from '@ionic/vue'
import { defineComponent } from 'vue'
import { getTicketList } from '@/http/modules/myPage'
import ticketDetail from './VoucherDetail.vue'

export default defineComponent({
  name: 'VoucherListPage',
  components: {
    IonPage
  },
  data() {
    return {
      title: '교환권/티켓',
      isTicket: true,
      avlbTickets: [],
      unavlbTickets: [],
      dtlInfo: {
        ordNo: '',
        ordDtlSeqNo: 0,
        tkNo: 0,
        prdtGbn: ''
      }
    }
  },
  ionViewWillEnter() {
    this.getTicketLists()
  },
  methods: {
    // 티켓/교환권 목록 조회
    getTicketLists() {
      getTicketList().then(res => {
        this.avlbTickets = res.data.avlbTickets
        this.unavlbTickets = res.data.unavlbTickets
        console.log(this.avlbTickets)
      }).catch(e => {
        console.log(e)
      })
    },
    clickedButton(val) {
      this.isTicket = val
    },
    // 티켓 상세로 이동
    goTicketDetail(info) {
      this.dtlInfo.ordNo = info.ordNo
      this.dtlInfo.ordDtlSeqNo = info.ordDtlSeqNo
      this.dtlInfo.tkNo = info.tkNo
      this.dtlInfo.prdtGbn = info.prdtGbn
      this.dtlInfo.isTicket = this.isTicket
      this.$router.push({ name: 'voucherDetail', query: this.dtlInfo })
    },

    // pc에서 모달창 띄우기
    async openModal(flag) {
      const components = {
        ticket: { title: '교환권/티켓', comp: ticketDetail, class: 'ticket-detail-h' }
      const modal = await modalController
        .create({
           component: components[flag].comp,
           cssClass: ['pc-modal-sm', components[flag].class],
          componentProps: {
            popTitle: components[flag].title,
            popDetail: info,
            close: () => {
              modal.dismiss()
            }
          }
        })
      return modal.present()
    }
  }
})
</script>

```
+ 상세 페이지✨
+ `<PopupLayout :title="popTitle" :close="close">`: props로 넘겨준 값 받아서 사용 => 팝업창에 '교환권/티켓'이라는 제목이 보이고 close 버튼이 생김
+ `<template #pop-con>`: slot으로 상세페이지  template 넣어주기
+ `props: ['popTitle', 'close', 'popDetail']`: props로 받아서 
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
=======================================(생략)========================================
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
#### 잠깐❗❗❗ 근데 위에 처럼만 하면 팝업창 안에 template 내용이 제대로 나오지 않는 문제가 발생한다.
+ 웹버전일 경우 팝업창 상세페이지로 가는 router 설정을 제대로 해주지 않았기 때문이다. 
+ 리스트 페이지 수정
  + `@click="device === 'PC' ? 'openModal('ticket')' : 'goTicketDetail(ticket)'"` 이렇게 템플릿에서 분기를 나눠주지 말고 `@click="goTicketDetail(ticket)"`이렇게 모바일 버전일 경우 상세페이지로 가게 해둔다.
  + 스크립트 부분을 바꿔준다.
  ```node
  // 티켓 상세로 이동
    goTicketDetail(info) {
      this.dtlInfo.ordNo = info.ordNo
      this.dtlInfo.ordDtlSeqNo = info.ordDtlSeqNo
      this.dtlInfo.tkNo = info.tkNo
      this.dtlInfo.prdtGbn = info.prdtGbn
      this.dtlInfo.isTicket = this.isTicket
      if (this.device === 'PC') { // 여기서 PC/모바일 버전 분기를 쳐준다.
        this.openModal(this.dtlInfo) // PC일 경우 openModal() 함수를 타게 만든다.
      } else { // 모바일일경우 query를 통해 페이지가 이동하게 한다.
        this.$router.push({ name: 'voucherDetail', query: this.dtlInfo })
      }
    },

    // pc에서 모달창 띄우기
    async openModal(info) { // 오브젝트가 하나여서 flag를 사용할 필요가 없음. 그냥 info를 통해서 값을 받아옴
      const component = { title: '교환권/티켓', comp: ticketDetail, class: 'ticket-detail-h' } // 오브젝트가 하나여서 구지 components ={} 로 안 묶어도 됨
      const modal = await modalController
        .create({
          component: component.comp,
          cssClass: ['pc-modal-sm', component.class],
          componentProps: {
            popTitle: component.title,
            popDetail: info, // 위에서 받아온 것을 통해 상세페이지 팝업 뜸
            close: () => {
              modal.dismiss()
            }
          }
        })
      return modal.present()
    }
  }
  ```
+ 상세 페이지 수정
  + 스크립트 수정
  ```node
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
  ```
  + 웹페이지 버전일 때에는 `ionViewWillEnter()`을 타지 않기 때문에 created에도 호출해 주었다.








