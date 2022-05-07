#### PC버전일 경우 모달창 띄우기(API 연동)
+ 모바일 버전에서는 클릭 시 해당 페이지로 이동하는 형태이지만 웹페이지 버전에서는 해당 페이지가 팝업으로 띄어지는 방식으로 구현
+ 웹버전 모달창 띄우기
+ 리스트 페이지(교환권/티켓) => 교환권을 클릭하면 해당 디테일 페이지 팝업창이 뜬다.
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
          componentProps: { // 이것들을 상세페이지에 props로 넘겨줌
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
