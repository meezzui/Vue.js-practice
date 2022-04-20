#### dataloading 걸기
+ `dataloading`이란?
  + 일반적으로 우리가 생각하는 로딩화면이라고 생각하면 된다.
  + `this.showLoading()` 로 로딩 화면을 시작하고 데이터를 불러오기 전에 보통 로딩화면을 띄우고 데이터를 다 불러오면 `this.dismissLoading()` 로 로딩화면을 없앤다.
  + `this.dismissLoading()` 두 번 해주는 이유🧨 : 두번 해주지 않으면 기존 창이랑 에러메시지가 동시에 뜨게된다.
+ 방법(2가지)
  + `try-catch문`으로 묶기(async 일때 사용)
  + `try-catch`에는 `finally`를 사용할 수 없음
  ```node
  async getProductInfo() {
    try {
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
    }
  }
  ```
  + `.then ~ finally`로 묶기(finally를 쓸 수 있어서 더 간편)
  + `finally`는 마지막에 무조건 실행되는 애니까 `finally`에만 `this.dismissLoading()`을 해주면 된다.
  ```node
  // finally 안 쓴 버전
  getMyPageInfo() {
    getMyPageMain().then(res => {
      this.dismissLoading()
      this.mainInfo = res.data
    }).catch(e => {
      this.dismissLoading()
    })
  }
  
  ============================================
  // finally 쓴 버전
  
  getMyPageInfo() {
    getMyPageMain().then(res => {
      this.mainInfo = res.data
    }).catch(e => {
    }).finally(_ => {
      this.dismissLoading()
    })
  }
  
  ```
















