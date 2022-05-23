#### 다른 페이지를 갔다 오면 같은 데이터가 똑같이 또 들어가 있는 이슈 해결
+ 예를들어, 찜 목록에 찜이 된 데이터가 3개가 있을 경우, 다른 페이지를 갔다 다시 찜 목록을 들어가보면 똑같은 데이터가 두번 들어가는 문제가 발생하였다.
+ 그 이유는??😯
  + `ionViewWillEnter()` 메소드 안에 `this.getFavorList()` 때문이다.
  + `ionViewWillEnter()`는 해당 페이지에 진입했을 경우 화면에 그려지는 메소드이다.
  + 그렇기 때문에 다른 페이지를 들렸다가 이 페이지에 진입했을 경우 같은 화면을 또 한번 그리게 된다.
  + 그래서 똑같은 데이터가 한번 더 들어가게 되는 문제가 발생하는 것이다.

#### 이를 해결하기 위해선 어떻게 해야할까??🤔
+ `ionViewWillEnter()` 메소드 안에 `this.getFavorList()`을 타기 전에 리스트를 한번 초기화 해주면 된다.
+ 초기화 함수는 `methods:{}` 안에 함수를 적어주고 그것을 `ionViewWillEnter()` 메소드 안에 다시 선언해주면 된다.
```node
data() {
    return {
      title: '찜한 상품',
      formClass: ['search_bar_wrap'],
      categoryList: [
        { key: '', name: this.$t('store'), list: [], page: 1 },
        { key: '', name: this.$t('product'), list: [], page: 1 },
        { key: '', name: this.$t('tourPass'), list: [], page: 1 },
        { key: '', name: this.$t('tourInfo'), list: [] }
      ],
      req: { target: '', tagGbn: 'F', size: 2, page: 1 },
      cartParams: {}
    }
  },
  async ionViewWillEnter() {
    this.reset()
    this.getFavorList()
  },
  methods: {
    reset() {
      // 리스트 초기화
      this.categoryList = [
        { key: '', name: this.$t('store'), list: [], page: 1 },
        { key: '', name: this.$t('product'), list: [], page: 1 },
        { key: '', name: this.$t('tourPass'), list: [], page: 1 },
        { key: '', name: this.$t('tourInfo'), list: [] }
      ]
    }
   }
```
+ 해당 데이터는 `list:[]` 안에 들어가기 것이기 때문에 초기화를 할 때 `getFavorList()` 그냥 이 함수를 초기화하면 초기화 되지 않는다.
+ 따라서 위에 `data()`부분에 선언해준 방식(api연동 형태)대로 해줘야 한다.
+ 저렇게 초기화를 해주고 `getFavorList()`를 타기 전에 초기화 함수로 선언한 `reset()`을 써주면 해당 페이지에 진입했을 때 리스트가 한번 초기화되고 화면이 그려지게 된다.




