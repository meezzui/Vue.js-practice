#### 생명주기 

<img width="400" src="https://user-images.githubusercontent.com/86812098/163714794-e5600bdc-58e7-4b7f-a6a7-8ab4b807ed8c.png">

#### ionic 이벤트 메소드(생명주기)
+ `ionViewDidLoad()`: 처음 페이지가 로딩되었을때, 캐시된 페이지에서는 동작을 하지 않음.
+ `ionViewWillEnter()`: 페이지에 들어가서, 페이지가 활성활 되었을때 실행
+ `ionViewDidEnter()`: 페이지에 완전히 접근하여 활성화가 되었을때, 캐시 여부와 관계없이 실행 됌.
+ `ionViewWillLeave()`:	페이지를 막 떠나기 시작할때 실행
+ `ionViewDidLeave()`: 페이지 떠나기를 완료하였을 때 실행
+ `ionViewWillUnload()`: 페이지가 제거 돼고, 페이지의 엘리먼트가 제거 됐을때 실행

🎈 `created()`에 선언한 것도 `ionViewWillEnter()`에도 써줘야 함. 🎈
+ `ionViewWillEnter()` 여기에 써주지 않으면 실행이 안 되는 기종도 있기 때문에 둘다 써줘야 함
+ 예시
```node
ionViewWillEnter() {
  this.frcsPrdtNo = this.$route.params.frcsPrdtNo
  this.getProductInfo()
  this.getReviewList()
},
created() {
  this.frcsPrdtNo = this.$route.params.frcsPrdtNo
  console.log(this.frcsPrdtNo)
  this.getProductInfo()
  this.getReviewList()
}
```

