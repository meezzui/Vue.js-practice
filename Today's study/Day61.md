#### 상세 페이지 갔다가 뒤로 가기 했을 시 카테고리는 '전체'에 가 있는데 데이터는 그렇지 않는 문제
+ `list.push(data)`로 카테고리별로 데이터를 각각 넣어주었다.
+ 그런데 문제점이 발생했다..💥
  + 이렇게 각각 카테고리에 데이터를 `push`해주다 보니 해당 데이터를 클릭해서 해당 상품의 상세 페이지를 갔다가 뒤로 가기를 눌러서 다시 찜 목록 페이지로 오면 `'전체'`에 가 있는데 데이터는 그렇지 않는 문제가 발생했다.
  + 왜 이런 문제가 발생한 걸까??😯
    + 다른 페이지에 갔다 오면 데이터는 `'전체'` 카테고리의 데이터가 잘 들어온다.
    + 그러나 각각의 카테고리에 `list`의 데이터를 `push`해주고 있기 때문에 상세 페이지를 들어가기 전에 선택 되었던 카테고리의 데이터가 보이게 된다.

+ 이를 해결하려면 어떻게 해야할까??😲
  + 각 카테고리에 데이터를 `push`하기 전, 다시말해 각 카테고리에 데이터를 `push`하는 함수의 최상단에 `'전체'` 카테고리가 선택되게 한다.
  + 그러면 데이터가 셋팅 되기 전에 항상 카테고리가 `'전체'`로 선택되어 다른 페이지를 갔다가 왔을 경우 `'전체'`에 해당하는 데이터를 보여주게 된다.

```node
// 위에 템플릿 단에서 카테고리에 해당하는 input태그의 v-model 값을 `selectedCategory`라고 주었다.

data() {
  return {
    categoryList: [
      { key: 'ALL', name: this.$t('all'), list: [], page: 1 },
      { key: 'STORE', name: this.$t('storeTitle'), list: [], page: 1 },
      { key: 'PRODUCT', name: this.$t('product'), list: [], page: 1 },
      { key: 'TOURPASS', name: this.$t('tourPass'), list: [], page: 1 },
      { key: 'COURSE', name: this.$t('tourInfo'), list: [] }
    ],
    selectedCategory: 'ALL' // 카테고리의 default값은 '전체'
  }
  },
  async ionViewWillEnter() {
  this.reset()
  this.getFavorList()
  },
  methods: {
   //리스트 초기화
  reset() {
    this.categoryList.forEach(c => {
      c.list = []
    })
  },
  // 해당 카테고리의 데이터를 list에 넣어주는 함수
  async getFavorList() {
    this.selectedCategory = 'ALL' // 이렇게 맨 처음에 선언해줘서 해당 상품의 상세 페이지를 갔다왔을 경우 다시 카테고리가 '전체'로 선택되게 한다.
    await getFavorites().then(res => {
      console.log(res)
      res.data.stores.forEach(s => {
        const data = {
          stORCtgr: s.ctgrNm,
          img: s.tmnlImgUrl,
          fvrVal: s.frcsId,
          mainTitle: s.frcsNm,
          rating: s.rvScr,
          fvrYn: s.fvrYn,
          rvCnt: s.rvCnt,
          tags: s.tagVals,
          fvrGbn: 'F'
        }
        this.categoryList[1].list.push(data)
        this.categoryList[0].list.push(data)
      })

      res.data.frcsPrdts.forEach(s => {
        const data = {
          img: s.tmnlImgUrl,
          fvrVal: s.frcsPrdtNo,
          mainTitle: s.frcsPrdtNm,
          rating: s.rvScr,
          rvCnt: s.rvCnt,
          DcYn: s.frcsDcYn,
          DcVal: s.frcsDcVal,
          originPrice: s.frcsNmlAmt,
          salesPrice: s.frcsSalAmt,
          fvrYn: s.fvrYn,
          fvrGbn: 'P',
          stORCtgr: s.ctgrNm,
          tags: s.tagVals
        }
        this.categoryList[2].list.push(data)
        this.categoryList[0].list.push(data)
      })
==========================================(생략- 위 형태 반복)============================================
      console.log('test', this.categoryList)
    }).catch(e => {

    })
  }
```
