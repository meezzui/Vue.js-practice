#### 옵션 선택 배송비 수정사항
+ 옵션을 선택한 경우 배송비가 있는건 배송비가 추가되어서 보여져야 하는데 배송비가 두번 추가되어진다...
+ 수정 전
  + 옵션이 없을 경우 `this.setOrderOption()`으로 인해 `setOrderOption()`이 실행된다.
  ```node
  if (this.productDetail.frcsPrdtTp !== 'V') {
      // 상품 유형이 '현장교환이 아닌 경우' 총 금액에 배송비 포함
      this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt) + this.productDetail.dlvyAmt
    } else {
      // 상품 유형이 '현장교환인 경우' 총 금액에 비송비 미포함
      this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt)
    }
  ```
  + 위에 코드에서 이미 구매타입이 'V'가 아닌 경우 배송비를 추가해줬다. 
  + 그런데 `<p v-if="productDetail.frcsPrdtTp !== 'V'"><span>{{addComma(totalPrice + productDetail.dlvyAmt)}}</span>&nbsp;원</p>` 이렇게 배송비를 또 추가해줘서 배송비가 두번 추가된 것이다.
  ```node
  <template>
    <div class="total_price_box">
      <div class="total_price_p">
        <p>총 주문금액</p>
      </div>
      <div class="total_price">
        <p v-if="productDetail.frcsPrdtTp !== 'V'"><span>{{addComma(totalPrice + productDetail.dlvyAmt)}}</span>&nbsp;원</p>
        <p v-else><span>{{addComma(totalPrice)}}</span>&nbsp;원</p>
      </div>
    </div>
  </template>

  <script>
  // 상품 상세
  async getProductInfo() {
    try {
      this.showLoading()
      const res = await getProductInfo(this.frcsPrdtNo)
      console.log(res)
      this.productDetail = res.data.prdctDetail
      this.purchaseInfo = res.data.salpsInfo

      // 옵션이 없는 경우
      if (this.productDetail.optSets.length === 0) {
        console.log('옵션이 없습니다!!')
        this.setOrderOption()
      } else {
        // 옵션이 있는 경우
        this.setDefaultOption()
      }
      this.dismissLoading()
    } catch (error) {
      console.log(error)
      this.dismissLoading()
    },
  // 구매 옵션 셋팅
    setOrderOption() {
      this.orderOpt = []
      this.totalPrice = 0
      this.orderOpt.push({
        optNo: 0,
        cnt: 1,
        optNm: this.productDetail.frcsPrdtNm
      })
      if (this.productDetail.frcsPrdtTp !== 'V') {
        // 상품 유형이 '현장교환이 아닌 경우' 총 금액에 배송비 포함
        this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt) + this.productDetail.dlvyAmt
      } else {
        // 상품 유형이 '현장교환인 경우' 총 금액에 비송비 미포함
        this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt)
      }
    }
  </script>
  ```
+ 수정후
  + `<p v-if="productDetail.frcsPrdtTp !== 'V'"><span>{{addComma(totalPrice + productDetail.dlvyAmt)}}</span>&nbsp;원</p>` 이 부분을 빼주고 if문을 없애줬다.
```node
<template>
  <div class="total_price_box">
    <div class="total_price_p">
      <p>총 주문금액</p>
    </div>
    <div class="total_price">
      <p><span>{{addComma(totalPrice)}}</span>&nbsp;원</p>
    </div>
  </div>
</template>

<script>
// 상품 상세
async getProductInfo() {
  try {
    this.showLoading()
    const res = await getProductInfo(this.frcsPrdtNo)
    console.log(res)
    this.productDetail = res.data.prdctDetail
    this.purchaseInfo = res.data.salpsInfo

    // 옵션이 없는 경우
    if (this.productDetail.optSets.length === 0) {
      console.log('옵션이 없습니다!!')
      this.setOrderOption()
    } else {
      // 옵션이 있는 경우
      this.setDefaultOption()
    }
    this.dismissLoading()
  } catch (error) {
    console.log(error)
    this.dismissLoading()
  },
// 구매 옵션 셋팅
  setOrderOption() {
    this.orderOpt = []
    this.totalPrice = 0
    this.orderOpt.push({
      optNo: 0,
      cnt: 1,
      optNm: this.productDetail.frcsPrdtNm
    })
    if (this.productDetail.frcsPrdtTp !== 'V') {
      // 상품 유형이 '현장교환이 아닌 경우' 총 금액에 배송비 포함
      this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt) + this.productDetail.dlvyAmt
    } else {
      // 상품 유형이 '현장교환인 경우' 총 금액에 비송비 미포함
      this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt)
    }
  }
</script>
```
















