### 오늘의 일기
---
#### 티켓 미사용/사용 완료 및 만료
+ `클래스 바인딩`과 `v-if/v-else`를 사용한다.
+ 코드 설명
  + `완료/만료` 탭을 선택하면 `isTicket`이 `false`로 바뀐다. 
  + `ticket-contents-box`에 `:class="isTicket ? '' : 'expire'"`을 보면 `isTicket`이 `false`일 경우 `expire클래스`의 스타일이 동작한다.
  + `스타일 부분`을 보면 `expire 클래스`가 동작할 경우 `.ti-period` 클래스의 `margin-left: 0;`이 적용되는 것을 볼 수 있다.
  +  `티켓 날짜 부분`에서 `v-if="isTicket"`울 사용하여 `isTicket`이 `false`일 경우 남은 일 수가 보이지 않게 하였다.
  +  또한 `사용여부 부분`을 보면 `isTicket`이 `true`일 경우는 티켓에 '사용하기'가 보이게 하였으며 `[완료/만료]` 탭을 선택할 경우 그 티켓의 `완료/만료` 여부를 보여주게 하였다.
  +  티켓의 `완료/만료` 여부는 그 안에 `template`으로 한번 더 묶어줘서 따로 빼주었다.
```node
<template>
<!-- 미사용,사용 완료/만료 선택 부분 -->
<div class="ticket-unuse">
  <button :class="isTicket ? 'ticket-unuse-tab on' : 'off'" @click="clickedButton(true)">미사용</button>
</div>
<div class="ticket-use">
  <button :class="!isTicket ? 'ticket-use-tab on':'off'"  @click="clickedButton(false)">완료/만료</button>
</div>
<!-- 컨텐츠 부분 -->
<div class="ticket-contain">
  <div class="ticket-contents-box box-margin" v-for="(items,i) in [1,1,1,1,1,1,1]" :key="i" :class="isTicket ? '' : 'expire'" @click="routeTo('voucherDetail')">
    <!-- 티켓 날짜 -->
    <div class="ticket-date-box">
      <div v-if="isTicket" class="ticket-remaining-period">
        <span class="ti-remain">29일 남음</span>
      </div>
      <div class="ticket-period">
        <span class="ti-period">2021.12.31까지</span>
      </div>
    </div>
    <!-- 사용여부 -->
    <div class="using-ticket">
      <p v-if="isTicket" class="using">사용하기</p>
      <template v-else>
        <p v-if="true" class="using-complete">사용완료</p>
        <p v-else class="period-complete">기간만료</p>
      </template>
    </div>
  </div>
</div>
</template>

<style lang="scss" scoped>
.ticket-contain{
  .expire{
    .ti-period{margin-left: 0;}
  }
}
</style>

<script>
data() {
  return {
    title: '교환권/티켓',
    isTicket: false
  }
},
methods: {
  clickedButton(val) {
    this.isTicket = val
}
</script>
```



