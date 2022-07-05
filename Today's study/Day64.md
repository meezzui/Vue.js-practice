#### 외부 클릭 감지🔎
+ 팝업 외부를 클릭했을 때 팝업창이 꺼지는 기능 구현
+ 순서
  + 라이브러리 설치하기 : `npm install --save click-outside-vue3` 설치
  + 사용할 페이지에 import 하기 : `import vClickOutside from 'click-outside-vue3'`
  + 해당 팝업 요소에 `v-click-outside=""` 추가 `""`안에는 원하는 이름을 지어주어 넣기 => `v-click-outside="onClickOutside"`
  + 여기서 주의❗❗❗
    + `directives:{}`를 꼭 추가해줘야 한다.
    ```node
    directives: {
      clickOutside: vClickOutside.directive
    }
    ```
  + `data(){}` 안에 클릭했을 때 팝업이 뜨게 보여지는 값을 `false`로 선언해준다.
  ```node
  <template>
    <div @click="myInfo()">{{$t('myPage')}}</div> // 클릭 시
    <div v-if="isMyInfo" v-click-outside="onClickOutside" class="myInfo_popup"> // 팝업창 나타남
    ======================================(생략)==================================
    </div>
  </template>
  
  data(){
    return{
      isMyInfo: false
    }
  }
  
  <script>
    methods: {
    // 클릭 시 isMyInfo값 변환해주는 메소드 => @click="myInfo()" 이 부분을 클릭하면 열림/닫힘
      myInfo() {
      this.isMyInfo = !this.isMyInfo
    }
   }
  </script>
  ```
  + 외부를 클릭 했을 때 팝업창 닫히게 하기
  ```node
  onClickOutside() {
    this.isMyInfo = false
  }
  ```
  + 외부 클릭 시 `isMyInfo`의 값을 `false`로 바꿔주어서 팝업창이 닫히게 한다. 
  + HOW❓❓ 팝업창이 열릴려면 `isMyInfo` 값이 `true`가 되어야 하기 때문에 `isMyInfo` 값을 `false`로 바꿔주면 팝업창이 사라지게 된다.
  
  
  
  
  
  
  
  
  
