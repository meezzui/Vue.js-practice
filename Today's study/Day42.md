#### 상품 구매 옵션  
[template 코드 설명]
+ `:modalHeight="selectedOpt.length > 1 ? '442px' : '390px'"`: 옵션리스트에 값이 있을 때와 없을때 모달창 height값 지정
+ `<article class="product-selectbox-wrapper" v-if="optionSet.length !== 0">` : 옵션이 있을때만 노출 
+ `<div v-for="(optSet, idx) in optionSet" :key="idx">` : select박스를 하나만 for문을 돌림!!(키포인트🧨) 하나만 돌려서 그에 해당하는 하위 옵션값들이 저절도 셋팅되게 함
+ `<select v-model="selectedOpt[idx]" required @change="setOptions(idx)">`: 선택된 옵션의 idx값을 주고 받음
+ `<option value="" disabled>{{ optSet.name }}</option>` : placeholder기능이 안 되서 이 option태그만 for문을 돌리지 않고 기본 들어오는 값으로 넣어줬다.
```
<option
  v-for="val in optSet.values"
  :key="val"
  :label="val"
  :value="val">{{ val }}</option>
```
+ option태그를 for문 돌려서 optSet에 들어오는 value값들이 들어올 수 있게 했다.
+ `<article v-for="(selected , s) in orderOpt" :key="s" class="selected-product-wrapper">` : 선택된 옵션 반복문으로 돌리기

+ `<article v-for="(selected , s) in orderOpt" :key="s" class="selected-product-wrapper">` : 선택된 옵션 리스트
+ `<Count :count-option="{count: selected.cnt, index1: s}" @change="onChangeCount"/>` : 옵션 수량부분. Count.vue컴포넌트로 따로 빼둠. 해당 인덱스의 선택된 수량만큼 들어감
+ `{{ addComma(selected.cnt * tourpassInfo.salAmt) }}`: (선택된 옵션의 수량 * 상품 가격)을 나타냄
+ `<article v-if="orderOpt.length !== 0">`: 선택된 리스트에 값이 있을 경우에만 총 주문금액 노출  

[script 설명]  
+ 아래 스크립트 부분에 주석으로 설명 달아놓음
```node
<template>
  <ion-page>
    <BaseLayout id="tour_pass_detail" :header="{title:'투어패스 상세', back:true, cart: true}">
      <template #con>
================================================(생략)==============================================
    <!--구매 모달 창-->
    <BaseModal :modalState="showModal" :modalHeight="selectedOpt.length > 1 ? '442px' : '390px'" @close="showModal = false">
      <template #modal-con>
        <dl class="modal-contents">
          <div class="modaltouchbar">
            <img src="@/assets/image/modaltouchbar.png" @click="selectedOpt.push({})">
          </div>
          <section class="content-padding-s modal-scroll">
            <form>
              <article class="product-selectbox-wrapper" v-if="optionSet.length !== 0">
                <div v-for="(optSet, idx) in optionSet" :key="idx">
                  <select v-model="selectedOpt[idx]" class="product-option-selectbox" style="border-bottom:1px solid #D1D3D7;" required @change="setOptions(idx)">
                    <option value="" disabled>{{ optSet.name }}</option>
                    <option
                        v-for="val in optSet.values"
                        :key="val"
                        :label="val"
                        :value="val">{{ val }}</option>
                  </select>
                </div>
              </article>
              <article v-for="(selected , s) in orderOpt" :key="s" class="selected-product-wrapper">
                <div class="selected-product" v-if="orderOpt.length > 0">
                  <ion-row>
                    <ion-col style="font-size:14px;" size="11">
                      {{ selected.optNm }}
                    </ion-col>
                    <ion-col v-if="optionSet.length !== 0" style="text-align:right" size="1" @click="handleDeleteItem(s)">
                      <img style="width:12px;" src="@/assets/image/icon-x-g.png">
                    </ion-col>
                  </ion-row>
                  <ion-row style="margin-top:5px;">
                    <ion-col size="3">
                      <Count :count-option="{count: selected.cnt, index1: s}" @change="onChangeCount"/></ion-col>
                    <ion-col style="text-align:right; font-size:16px;" size="9"><span style="font-weight:bold;">{{ addComma(selected.cnt * tourpassInfo.salAmt) }}</span>원</ion-col>
                  </ion-row>
                </div>
              </article>
              <article v-if="orderOpt.length !== 0">
                <ion-row>
                  <ion-col size="9" style="font-size:14px; color:#717377;">총 주문금액</ion-col>
                  <ion-col size="3" style="font-size:18px; color:#EF3F3E; text-align:right; "><span style="font-size:20px;font-weight:bold;">{{ addComma(totalPrice) }}</span> 원</ion-col>
                </ion-row>
              </article>
            </form>
          </section>
        </dl>
        <BottomButton class="bottom-button" left="담기" right="바로구매" isValid="false" isPadding= "true" @click-left="onPutCart()"/>
      </template>
    </BaseModal>
  </ion-page>
</template>

<script>
export default {
  data() {
    return {
      tabList: [true, false, false],
      tourReviewList: [],
      productDetail: '',
      cpxPrdtNo: '',
      tourpassInfo: [],
      selectedList: [{}],
      selectedOpt: [],
      orderOpt: [],
      optionSet: [],
      options: [],
      selected: '',
      totalPrice: 0,
      showModal: false,
      reviewParams: {
        end: 0,
        page: 1,
        size: 10,
        start: 0,
        rvGbnVal: '',
        rvGbn: 'T',
        orderGbn: ''
      },
      fvrStatus: {
        fvrVal: '',
        fvrGbn: ''
      }
    }
  },
  created() {
    this.cpxPrdtNo = this.$route.params.cpxPrdtNo
    this.getTourpassInfo()
    this.getReviewList()
  },
  ionViewWillEnter() {
    this.cpxPrdtNo = this.$route.params.cpxPrdtNo
    this.getTourpassInfo()
    this.getReviewList()
  },
  methods: {
    // 투어패스 상세
    async getTourpassInfo() {
      const res = await getTourpassInfo(this.cpxPrdtNo)
      console.log(res)
      this.tourpassInfo = res.data

      if (this.tourpassInfo.optSets.length === 0) {
        // 옵션 없는 경우 구매 데이터 세팅
        console.log('옵션 없음!!')
        this.setOrderOption()
      } else {
        // 옵션 있는 경우 옵션 초기 기본값 세팅
        this.setDefaultOptions()
      }
    },
    clickedTab(idx) {
      for (let i = 0; i < this.tabList.length; i++) {
        this.tabList[i] = false
      }
      this.tabList[idx] = true
      console.log(this.tabList)
    },
    handlePayment() {
      this.showModal = true
    },
    // 옵션 상품 삭제
    handleDeleteItem(s) {  // x 버튼을 누르면 해당 옵션상품이 삭제 됨
      this.totalPrice = this.totalPrice - (this.orderOpt[s].cnt * this.tourpassInfo.salAmt) // 총 금액도 달라지겠지?? 해당 총 금액에서 그 해당 옵션상품 금액을 빼줌
      this.orderOpt.splice(s, 1) // 하나씩 삭제하기
    },
    onPutCart() {
      this.$alert({
        title: '장바구니에 상품이 담겼습니다.',
        left: '계속쇼핑',
        right: '장바구니 이동',
        img: 'bascket'
      })
    },
    // 옵션 상품 갯수
    onChangeCount({ count, index1 }) { // Count 컴포넌트로 따로 빼둔거 사용하는 방법
      this.totalPrice = 0 // data()에 totalPrice선언해준거 0으로 잡기
      this.orderOpt[index1].cnt = count // 여기서 cnt는 setOrderOption()에서의 cnt를 말함
      for (let i = 0; i < this.orderOpt.length; i++) {
        this.totalPrice = this.totalPrice + (this.orderOpt[i].cnt * this.tourpassInfo.salAmt) //총 금액 = 옵션 갯수 * 상품 
      }
    },
    // 초기 옵션 기본 세팅
    setDefaultOptions() {
      // 옵션을 위한 데이터 세팅
      this.selectedOpt = []
      this.options = this.tourpassInfo.prdtOpts // 옵션 목록
      this.optionSet = this.tourpassInfo.optSets // 옵션 값 정보

      // 옵션 없는 optionSet 데이터 제거
      this.optionSet = this.optionSet.filter((opt) =>
        opt.name !== ''
      )

      // optionSet 데이터 가공
      for (let i = 0; i < this.optionSet.length; i++) {
        console.log(i)
        // 첫번째 옵션을 제외한 옵션 value는 빈값 세팅
        if (i !== 0) {
          this.optionSet[i].values = []
        }
        // select box 옵션 항목 표시를 위해 selectedOpt에 옵션 개수만큼 빈 데이터 세팅
        this.selectedOpt.push('')
      }
    },
    // 옵션 값 변경 시
    setOptions(idx) {
      // 옵션 선택 완료
      if (idx + 1 === this.optionSet.length) {
        this.options.some((opt) => {
          let targetYn = true
          for (let i = 0; i <= idx; i++) {
            const name = 'optVal' + (i + 1)

            if (opt[name] !== this.selectedOpt[i]) {
              targetYn = false
              break
            }
          }
          // 선택했던 selectBox 초기화
          // this.selOpt = []
          if (targetYn) {
            // 중복 체크
            const already = this.orderOpt.some(x => (x.optNo === opt.optNo))
            if (!already) {
              const result = {
                optNo: opt.optNo,
                cnt: 1,
                optNm: ''
              }
              const optNm = []
              for (let i = 1; i <= 3; i++) {
                const optVal = 'optVal' + i
                if (opt[optVal] !== '') {
                  optNm.push(opt[optVal])
                }
              }
              result.optNm = optNm.join('/')
              this.orderOpt.push(result)
              this.totalPrice = this.totalPrice + this.tourpassInfo.salAmt
            } else {
              this.$alert({
                msg: '이미 선택된 옵션입니다.',
                middle: '확인'
                // success: () => {
                //   this.showModal = true
                // }
              })
            }
            this.setDefaultOptions()
            return true
          }
        }, {})
      } else {
        // 상위 옵션 값 변경 시 선택한 하위 옵션들 초기화
        console.log(idx)
        if (this.selectedOpt.length > 1) {
          this.selectedOpt = this.selectedOpt.reduce((temp, select, selectIdx) => {
            if (selectIdx > idx) {
              select = ''
            }
            temp.push(select)
            return temp
          }, []) // temp에 빈 배열 선언
          // 상위 옵션 값 변경 시 하위 옵션 목록 초기화
          this.optionSet = this.optionSet.reduce((temp, opt, optIndex) => {
            if (optIndex > idx) {
              opt.values = []
            }
            temp.push(opt)
            return temp
          }, [])
        }
        // 다음 옵션 값 추가
        this.optionSet[idx + 1].values = this.options.reduce((temp, opt) => {
          // 선택한 상위 옵션 값에 맞는 옵션인지 체크
          let optionYn = true

          for (let i = 0; i <= idx; i++) {
            const optVal = 'optVal' + (i + 1)

            if (opt[optVal] !== this.selectedOpt[i]) {
              optionYn = false
              break
            }
          }

          // 선택한 상위 옵션 값에 맞는 옵션이면 다음 옵션 데이터에 추가
          if (optionYn) {
            temp.push(opt['optVal' + (idx + 2)])
          }

          return [...new Set(temp)]
        }, [])
      }
      console.log(this.selectedOpt)
    },
    setOrderOption() {
      this.orderOpt = []
      this.totalPrice = 0
      this.orderOpt.push({
        optNo: 0,
        cnt: 1,
        optNm: this.tourpassInfo.prdtNms
      })
      this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.tourpassInfo.salAmt)
    }
  }
}
</script>
```




