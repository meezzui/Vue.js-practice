### 오늘의 일기
---
+ var은 <script> 안에서 변수로 선언
+ let은 <script>안에서 변수로 한 번만 선언되어야 한다.
+ const는 <script> 안에서 변수로 한 번만 선언되고 값은 변경할 수 없다.

  
#### 메뉴 탭 버튼 클릭시 글자 색상과 밑에 언너바 생기게 하기
```node
<div class="title-event">
  <button :class="isEvent ? 'event-tab on' : 'off'" @click="clickedButton(true)">이벤트</button>
</div>
<div class="title-notice">
  <button :class="!isEvent ? 'notice-tab on':'off'"  @click="clickedButton(false)">공지사항</button>
</div>

 
<style lang="scss" scoped>
.on{border-bottom: 1.5px solid #22BDB6; color: #22BDB6; font-size: 14px;} // border-bottom 으로 언더바 생성
.event-tab, .notice-tab{height: 30px;} //header라인이랑 맞게 button태그 height를 줌
</style>
  
<script>
data() {
  return {
    isEvent: true
  }
},
methods: {
  clickedButton(val) {
    this.isEvent = val
  }
}
</script>
```
+ `isEvent`를 `true`로 초기값으로 잡는다.
+ 이벤트 버튼의 클릭함수를 `true`로 놓는다. -> 버튼이 이미 눌린 상태로 보여짐( `#22BDB6` 이 색깔로 보여짐)
+ `clickedButton(val) { this.isEvent = val }` : 클릭했을 때 작동하는 함수 -> true/false를 넣어줌
+ `:class="isEvent ? 'event-tab on' : 'off'"` : 클래스 바인딩 -> `isEvent`가 true이면 클래스 `on`이 작동하고 fasle이면 클래스 `off`가 작동한다.
+ 여기서 중요한 점!!!🧨 공지사항은 반대로 작용하니까 클래스 바인딩할 때 `!isEvent` 이렇게 써줘야 한다.
  
  
