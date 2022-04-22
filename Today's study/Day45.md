#### 구매타입 별 총 금액(코드 간략화)
+ 수정전
  + `data(){}` 안에 라디오 버튼 값을 넣어줄라고 v-model을 하기 위해 `checkType: ''`을 선언해 주었다.
  + 라디오 버튼이 왼쪽부분(현장교환)이 체크되어있게 하기 위해 `checkType: 'V' 이렇게 해주었다.
  ```node
    data() {
    return {
     ==================(생략)=================
      checkType: 'V'
      }
    }
  
  // 구매 옵션 셋팅
  setOrderOption() {
    this.orderOpt = []
    this.totalPrice = 0
    this.orderOpt.push({
      optNo: 0,
      cnt: 1,
      optNm: this.productDetail.frcsPrdtNm
    })
    this.setTotalPrice()
  },
  
  // 구매타입별 총 금액
  setTotalPrice() {
    console.log('radio', this.checkType)
    this.totalPrice = 0
    if (this.productDetail.frcsPrdtTp === 'D') { // 핸즈프리인 경우 배송비 포함
      this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt) + this.productDetail.dlvyAmt
    } else if (this.productDetail.frcsPrdtTp === 'V') { // 현장교환인 경우 배송비 미포함
      this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt)
    } else { // 핸즈프리와 현장교환 선택인 경우
      if (this.checkType === 'V') { // 현장교환 - 배송비 미포함
        this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt)
      } else { // 핸즈프리 - 배송비 포함
        this.totalPrice = this.totalPrice + (this.orderOpt[0].cnt * this.productDetail.frcsSalAmt) + this.productDetail.dlvyAmt
      }
    }
  }
  
  // 상품 수량
  onChangeCount({ count, index }) {
      this.totalPrice = 0
      this.orderOpt[index].cnt = count
      // 상품 수량에 따른 총 가격( 이것보다 훨씬 길었음)
      for (let i = 0; i < this.orderOpt.length; i++) {
          this.totalPrice = this.totalPrice + (this.orderOpt[i].cnt * this.productDetail.frcsSalAmt)
        }
    }
   ```   
+ 수정후
  + `checkType: 'V' -> `checkType: ''` 변경
  + async 상품상세 부분에 `this.checkType = this.productDetail.frcsPrdtTp === 'C' ? 'V' : this.productDetail.frcsPrdtTp` 추가
  + `this.checkType = this.productDetail.frcsPrdtTp === 'C' ? 'V' : this.productDetail.frcsPrdtTp`
    + 구매타입이 'C' 일때 라디오 박스 체크하는게 보이는데 'V'타입의 라이도 박스가 맨 처음에 체크되어 있게 하기 위한 것

   ```node
   data() {
      return {
       ==================(생략)=================
        checkType: ''
        }
      }
    // 상품 상세
    async getProductInfo() {
      try {
        this.showLoading()
        const res = await getProductInfo(this.frcsPrdtNo)
        console.log(res)
        this.productDetail = res.data.prdctDetail
        this.purchaseInfo = res.data.salpsInfo
        this.checkType = this.productDetail.frcsPrdtTp === 'C' ? 'V' : this.productDetail.frcsPrdtTp

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
      }
    },
    // 구매타입별 총 금액
    setTotalPrice() {
      console.log('radio', this.checkType)
      this.totalPrice = 0

      for (let i = 0; i < this.orderOpt.length; i++) {
        this.totalPrice = this.totalPrice + (this.orderOpt[i].cnt * this.productDetail.frcsSalAmt)
      }
      if (this.checkType === 'D') { // 핸즈프리 - 배송비 포함
        this.totalPrice += this.productDetail.dlvyAmt
      }
    },
    // 상품 수량
    onChangeCount({ count, index }) {
      this.totalPrice = 0
      this.orderOpt[index].cnt = count
      // 상품 수량에 따른 총 가격
      this.setTotalPrice()
    }
   ```
    
    
