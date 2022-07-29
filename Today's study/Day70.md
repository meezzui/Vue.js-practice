#### 룰렛 돌리기
+ 버튼을 누르면 룰렛이 돌아가게 만들기
+ 룰렛과 시작 버튼 `template`단
  + 룰렛 시작 버튼을 누르면 `startLoulette()` 함수가 실행 됨
  ```node
  // 돌아가는 룰렛 부분
  <div class="roulette_event">
    <img :class="roulletteClass" src="@/assets/image/event-roulette.png" alt="룰렛">
  </div>
  // 룰렛 시작 버튼
  <div class="roulette_start_btn" @click="startLoulette()">
      <img src="@/assets/image/event-roulette-start-btn.png" alt="룰렛 시작">
  </div>
  ```
+ `css`단
  + `@keyframes`은 애니매이션 효과를 주어 css를 제어할 수 있다.
  + 어떤 모양에서 어떤 모양으로, 혹은 어디에서 어디로 이동할 지에 대해 정해줄 수 있다.
  + `@keyframes`에서 `!important` 속성을 이용한 정의는 모두 무시된다.
  ```node
  // 룰랫 돌리기
  @keyframes rotation {
  from { -webkit-transform: rotate(0deg); }
  to { -webkit-transform: rotate(7300deg); }
  }
  .rotation-item {
      animation: rotation 7s ease-in-out forwards;
  }
  ```
+ `script`단
  + data()에 추가 될 클래스가 들어갈 배열을 선언 => `roulletteClass: []`
  + `this.roulletteClass.push('rotation-item')`: 배열에 `rotation-item`라는 클래스 추가
  + `setTimeout()` 함수를 이용해서 룰렛이 7초 동안 돌게 한다.
  ```node
  data() {
    return {
      roulletteClass: []
    }
  },
  methods: {
    // 룰렛 시작 버튼 클릭 시 클래스 추가
    startLoulette() {
      this.roulletteClass.push('rotation-item')
      setTimeout(_ => {
      }, 7000)
    }
  }
  ```
