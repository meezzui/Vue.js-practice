#### select 박스 선택 후 아래 div 보이기
+ select박스를 클릭하고 쿠폰을 선택하면 아래 상품가격,할인가격 등이 보여진다.
+ 주의❗❗❗ `@click` 이벤트를 사용하면 안 된다. => 이렇게 하면 select박스를 클릭함과 동시에 잠깐 보여졌다가 사라진다.
+ `watch: {}` 생명주기를 이용한다.😎 
+ `watch 함수`는 데이터가 변경되었을 때 타는 함수이다. 즉, select박스의 값이 변경되면 이 함수를 타게 된다.
```node
data() {
  return {
    couponChoiceList: [{}, {}, {}, {}],
    copiedCouponChoiceList: [],
    selectedCpn: [],
  }
}
watch: {
  couponChoiceList: {
    deep: true, // 배열안에 object를 타기 위한 코드
    handler(array) { // handler 함수 사용. handler는 새로운 value와 기존의 value를 비교할 수 있는 함수이다.
      array.forEach((a, index) => { // 반복문으로 select의 value값을 비교해줌 
        if (a.selected !== 'placeholder') { // 선택된 것이 'placeholder'라는 value값이랑 다를 경우 if문을 탐
          if (a.selected !== this.copiedCouponChoiceList[index].selected) { // 
            a.clicked = true // clicked에 true 값을 넣어줌
          }
        } else { //선택된 것이 'placeholder'라는 value값이랑 같을 경우 else문을 탐
          a.clicked = false // clicked에 false 값을 넣어줌
        }
      })

      this.copiedCouponChoiceList = this.array // copiedCouponChoiceList에 기존 배열 복사함
    }
  }
}
```
#### 여기서 잘 생각해야 할 점❗❗❗
+ 위에 처럼 코드를 짜면 select박스를 클릭하고 쿠폰을 선택하면 아래 상품가격,할인가격 등이 보여지는 기능이 수행되지 않을 것이다.💢
+ `this.copiedCouponChoiceList = this.array` 이렇게 하면 계속 값이 `false`가 들어간다.
+ 콘솔을 찍어보니 새로운 value값이랑 기존의 value 값이 같게 들어가고 있었다.(그니까 계속 false로 나오지...😥)
+ 그러면 왜 값이 같게 들어가는 것일까??🧐🧐🧐
  + 배열은 같은 곳(주소값)을 바라보고 있기 때문에 데이터가 하나가 변경될 시 배열 전체가 변경된다.
  + 따라서 기존의 value 값과 새로운 value값이 같게 되는 것이다.
+ 이러한 문제를 해결하려면 어떻게 해야할까??🧐🧐🧐
  + 복사한 기존의 배열의 형태를 변형시켜주면 된다.
  + 그러면 새로 들어온 value값은 다른 주소에 저장되기 때문에 서로 비교가 가능하다.
  + 기존의 배열을 복사한 것의 형태를 변경하기 위한 코드 => `this.copiedCouponChoiceList = JSON.parse(JSON.stringify(array))`
  + 코드 설명✔
    + `JSON.stringify(array)`: 배열을 string타입으로 바꾼다.
    + `JSON.parse(JSON.stringify(array))`: string타입으로 바꾼 것을 다시 JSON형태로 변형해준다.

#### 전체 코드
+ `select 태그` 보면 `value`값을 `placeholder`로 해놔서 이걸로 `watch함수`에서 `if문`으로 비교한다. 
```node
<template>
  <ion-page>
      <BaseLayout :header="{title: '쿠폰 선택', close: true}">
        <template #con>
          <div id="coupon_choice_contain">
===================================================================(생략)===========================================================================
                <select v-model="choice.selected" name="product" id="select_box" :style="choice.selected === 'placeholder' ? 'color: #B2B2B2;' : ''">-->
                  <option value="placeholder">쿠폰 선택</option>
                  <option value="할인 쿠폰">10% 쿠폰</option>
                 </select>
                </div>
                <div  v-if="selectedCpn[i]" class="price_contain">
                  <div class="pro_price_box">
                    <div class="pro_price_p">
                      <p>상품가격</p>
                    </div>
                    <div class="pro_price">
                      <p><span>{{addComma(items.salAmt * items.prdtCnt)}}</span>원</p>
                    </div>
                  </div>
                  <div class="discount_box">
                    <div class="discount_p">
                      <p>할인금액</p>
                    </div>
                    <div class="discount_price">
                      <p>-<span>{{addComma(items.dcAmt)}}</span>원</p>
                    </div>
                  </div>
                  <div class="final_price_box">
                    <div class="final_price_p">
                      <p>최종금액</p>
                    </div>
                    <div class="final_price">
                      <p><span>{{addComma(items.salAmt * items.prdtCnt - items.dcAmt)}}</span>원</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div>
            <BottomButton left="취소" right="할인적용" :isValid="true" @click-left="this.$router.go(-1)" @click-right="applyCpn"/>
          </div>
        </template>
      </BaseLayout>
  </ion-page>
</template>

<script>
import { IonPage } from '@ionic/vue'
import { getOrderCoupons } from '@/http/modules/coupon'
export default {
name: '',
components: {
  IonPage
},
data() {
  return {
    cpnList: [],
    orderInfo: [],
    copiedCouponChoiceList: [],
    selectedCpn: [],
    cpnInfo: {
      cpnInfos: [],
      totPrice: 0
    }
  }
},
computed: {
  // buttonValidation() {
  //   return this.selectedCpn.some(cpn => cpn.cpnIsuNo)
  // }
},
watch: {
   couponChoiceList: {
     deep: true,
     handler(array) {
       array.forEach((a, index) => {
         if (a.selected !== 'placeholder') {
           if (a.selected !== this.copiedCouponChoiceList[index].selected) {
             a.clicked = true
           }
         } else {
           a.clicked = false
         }
       })

       this.copiedCouponChoiceList = JSON.parse(JSON.stringify(array))
     }
   }
},
ionViewWillEnter() {
  this.orderInfo = JSON.parse(this.$route.query.orderInfo)
  this.getCoupons()
},
methods: {
  // 적용 가능한 쿠폰 목록 조회
  async getCoupons() {
    // 데이터 세팅
    for (let i = 0; i < this.orderInfo.orderDtls.length; i++) {
      this.selectedCpn.push('')
      this.cpnInfo.cpnInfos.push({
        ctgrGbn: this.orderInfo.orderDtls[i].ctgrGbn,
        ctgrSeqNo: this.orderInfo.orderDtls[i].ctgrSeqNo,
        frcsId: this.orderInfo.orderDtls[i].frcsId,
        prdtGbn: this.orderInfo.orderDtls[i].prdtGbn,
        prdtNo: this.orderInfo.orderDtls[i].prdtNo,
        optValCtnt: this.orderInfo.orderDtls[i].optValCtnt,
        totPrice: this.orderInfo.orderDtls[i].salAmt * this.orderInfo.orderDtls[i].prdtCnt
      })
    }
    this.cpnInfo.totPrice = this.orderInfo.salAmt

    const res = await getOrderCoupons(this.cpnInfo)
    this.cpnList = res.data.avlbCpns

    console.log('orderInfo', this.orderInfo.orderDtls)
    for (let i = 0; i < this.cpnList.length; i++) {
      for (let j = 0; j < this.orderInfo.orderDtls.length; j++) {
        if (this.cpnList[i].prdtGbn === this.orderInfo.orderDtls[j].prdtGbn && this.cpnList[i].prdtNo === this.orderInfo.orderDtls[j].prdtNo && this.cpnList[i].optValCtnt === this.orderInfo.orderDtls[j].optValCtnt) {
          this.orderInfo.orderDtls[j].cpnInfo = this.cpnList[i].avlbCpns
          if (this.orderInfo.orderDtls[j].cpnIsuNo !== 0) {
            this.selectedCpn[j] = this.cpnList[i].avlbCpns.filter((c) => {
              return c.cpnIsuNo === this.orderInfo.orderDtls[j].cpnIsuNo
            })[0]
          }
        }
      }
    }
    console.log('이미 선택 되어있는 쿠폰', this.selectedCpn)
  },
  getCheckList() {
    this.couponChoiceList.forEach((c, i) => {
      c.clicked = false
      c.selected = 'placeholder'
    })
  },
  setCpnInfo(cpnInfo, idx) {
    let couponDc = 0
    const prdtInfo = this.orderInfo.orderDtls[idx]
    const totPrice = prdtInfo.salAmt * prdtInfo.prdtCnt
    // 정률/정액 쿠폰인지 구분 > R: 정률, A: 정액
    if (cpnInfo[idx].cpnDcGbn === 'R') {
      // 정률 쿠폰일 때
      // 할인 가격 = 상품 가격 * 상품 수량 / 할인 퍼센트
      couponDc = totPrice / cpnInfo[idx].cpnDcGbnVal

      // 할인 금액이 할인 제한 금액보다 많을 때 할인 제한 금액 적용
      if (couponDc >= cpnInfo[idx].cpnDcGbnLmt) {
        couponDc = cpnInfo[idx].cpnDcGbnLmt
      }
    } else {
      // 정액 쿠폰일 때
      // 선택한 상품 총 가격이 할인 가격보다 작으면 전액 할인
      if (cpnInfo[idx].cpnDcGbnVal > totPrice) {
        couponDc = totPrice
      } else {
        couponDc = cpnInfo[idx].cpnDcGbnVal
      }
    }
    // default 값인 '선택' 클릭 시 쿠폰 발급 번호, 할인 가격 0으로 세팅
    if (cpnInfo[idx] === '') {
      this.orderInfo.orderDtls[idx].cpnIsuNo = 0
      this.orderInfo.orderDtls[idx].dcAmt = 0
    } else {
      this.orderInfo.orderDtls[idx].cpnIsuNo = cpnInfo[idx].cpnIsuNo
      this.orderInfo.orderDtls[idx].dcAmt = couponDc
    }
  },
  applyCpn() {
    this.orderInfo.dcAmt = 0
    for (let i = 0; i < this.orderInfo.orderDtls.length; i++) {
      this.orderInfo.dcAmt += this.orderInfo.orderDtls[i].dcAmt
      // 상품 정보에 각각 세팅했던 쿠폰 목록 제거
      delete this.orderInfo.orderDtls[i].cpnInfo
    }
    console.log('쿠폰 최종', this.orderInfo)
    this.$router.push({ name: 'OrderList', query: { orderInfo: JSON.stringify(this.orderInfo) }})
  }
},
setup() {
  return {}
}
}
</script>

<style lang="scss" scoped>
#coupon_choice_contain{
==================================(생략)====================================
}
</style>
```






