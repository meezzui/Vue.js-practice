#### 스와이퍼 변경시 티켓 상테에 따라 배경색 변경
+ 사용전과 사용완료 상태에 따라 티켓의 배경색이 바뀌게 만든다.
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
.tourpass_compose{
  padding: 17px 15px;
  background-color: #4587D3;
  .tourpass_p{
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 1;
    -webkit-box-orient: vertical;
    p{
      font-size: 14px;
      color: #FFFFFF;
    }
  }
}
.ticket-deteil-contain{background-color: #509DF5; height: -webkit-calc(100vh - 64px);}
.box{display: flex;}
.number, .period{vertical-align: middle;}
.info{font-size: 14px; color: #CBE2FC; margin-left: 15px;}
.ti-info{font-size: 14px; color: #FFFFFF; margin-left: 15px;}
.place{margin-left: 28px;}
.ticket-backround{ @include background('~@/assets/image/temp/ticketLayout.png', 100% 100%, 0 0); width: 330px; height: 415px; margin: 20px auto; overflow: auto;}
.ticket-back-box{
  overflow: auto;
  }
.swiper {
  --bullet-background: black;
  --bullet-background-active: #fff;
  --swiper-pagination-bullet-width: 6px;
  --swiper-pagination-bullet-height: 6px;
  .swiper-pagination{
    position: unset;
    margin-top: 20px;
  }
}
.ti-detail-img{
  width: 185px; height: 147px; margin: 8.6px 0 17px 0;
  img{
    width: 100%;
    height: 100%;
  }
  }
.ti-detail-title{font-size: 12px; color: #717377; margin-bottom: 8.6px;}
.ti-detail-contents{width: 242px; font-size: 14px; @include limitTwoLine(); text-align: center; font-family: medium;}
.ti-detail-opt{width: 242px; font-size: 12px; @include limitTwoLine(); text-align: center; font-family: medium;}
.ti-detail-count{font-size: 12px; color: #717377;}
.ti-detail-count-number{font-size: 12px; color: #EF3F3E; margin-left: 3px; font-family: medium; align-items: center;}
.ti-detail-count-box{display: flex; margin-top: 4.5px; align-items: center;}
.ti-detail-box{display: flex; flex-direction: column; align-items: center; margin: 38px 0; position: relative;}
.ticket-bottom-box{display: flex;  position: absolute; bottom: -100px; width: 270px; justify-content: space-between;}
.tourTicket-bottom-box{bottom: -140px;}
.comment-img{width: 147px; height: 50px;}
.ticket-bottom-img1{height: 50px; margin-top: 8px;}
.ticket-bottom-img2{
  width: 66px;
  height: 66.25px;
  img{
    width: 100%;
    height: 100%;
  }
}
.changed{
  background-color: #9AA2AB; height: -webkit-calc(100vh - 64px);
  .info{color: #FFFFFF; opacity: 70%;}
}
.stamp-box-container {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  display: grid;
  .stemp-box{
    position: relative;  width: 95px; height: 95px;
    display: block;
    margin: auto;
    .ticket-status-img {
      position: absolute;
      img{
        width: 100%;
        height: 100%;
      }
    }
    .ticket-status-container {
      position: absolute;
      top: 0;
      left: 0;
      bottom: 0;
      margin: auto;
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      .ticket-status {
        transform: rotate(-28deg);

        > span, p {
          font-size: 12px;
          font-family: medium;
          color: #fff!important;
        }
        > span {
          font-weight: bold;
        }
        > p {
          font-weight: 600;
        }
      }
    }

  }
}
.detail-info-box{padding: 0 15px;}
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
