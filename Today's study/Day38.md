#### 즐겨찾기 등록 및 제거 api연동
+ 코드 설명
```node
data(){
  return{
    fvrStatus: {
        fvrVal: '', //즐겨찾기 대상 번호
        fvrGbn: '' //즐겨찾기 대상구분
      }
  }
}
// 즐겨찾기
onClickedLike(like) {
  this.fvrStatus.fvrVal = this.cpxPrdtNo //공통부분의 즐겨찾기 부분 이므로 해당 페이지 코드로 적어준다.
  this.fvrStatus.fvrGbn = 'T' // 대상구분
  console.log(this.fvrStatus)
  if (like) { // 'like' 라고 내가 임의로 이름을 지정해준다.
    registerFavorite(this.fvrStatus).then(res => { // 즐겨찾기 등록!! registerFavorite() 이 함수는 공통 js로 빼주었다. 그리고 fvrStatus 구조를 그대로 넣어줬다.
    }).catch(err => { // 에러날 경우
      if (err.status === 401) { //401에러일 경우 콘솔에 표시
        console.log(err)
      } else {
        this.$alert({ // 그 이외에 것은 에러메시지 띄우기
          msg: err.errorMessage,
          success: () => {
            this.like = false
            err.success ? this.$router.push({ name: err.success }) : ''
          }
        })
      }
    }).finally(_ => { // '_' 자리에 아무거나 넣어두 됨 예) '()' 이런거 
      this.getTourpassInfo()
    })
  } else {
    deleteFavorite(this.fvrStatus).then(res => { // 즐겨찾기 해제
    }).catch(err => {
      if (err.status === 401) {
        console.log(err)
      } else {
        this.$alert({
          msg: err.errorMessage,
          success: () => {
            this.like = false
            err.success ? this.$router.push({ name: err.success }) : ''
          }
        })
      }
    }).finally(_ => {
    })
  }
}
```
+ 공통 js부분 예시
```node
// 즐겨찾기 등록
export function registerFavorite(data) {// 데이터를 보냄
  const url = `${prefix}` + '/favor'
  return request.post(url, data, {}) // 등록이니까 post  // get일때는 data는 리턴 안 함
}
```
