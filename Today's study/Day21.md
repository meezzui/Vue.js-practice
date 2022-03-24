### 메일 인증(인증전송을 누르면 재전송으로 바뀌고 인증번호 입력란이 활성화 됨)
```node
<div class="comfirm-box">
  <button v-if="!sendEmail" @click="clickedSendEmail(true)">인증전송</button>
  <button v-else @click="clickedSendEmail(true)">재전송</button>
</div>
-------------------------- (생략) ----------------------------------
<div v-if="sendEmail" class="put-confirm-num-contain">
  <div class="input-email-box submit-email-box">
    <div class="input-box confirm-num">
      <input type="text" placeholder="인증번호 입력">
    </div>
    <Timer class="timer" :counter="180" :time="'03:00'" :show="TimerShow"></Timer>
    <div class="comfirm-box comfirm-submit">
      <button class="">인증확인</button>
    </div>
  </div>
  <div class="comfirm-message">
    <span>입력한 메일 주소로 인증번호가 발송되었습니다.</span>
  </div>
</div>

<script>
export default defineComponent({
  name: 'EmailChangePage',
  components: {
    IonPage
  },
  data() {
    return {
      TimerShow: true,
      sendEmail: false
    }
  },
  methods: {
    clickedSendEmail(val) {
      this.sendEmail = val
    }
  }
})
</script>
```
+ `@click="clickedSendEmail(true)` : 인증전송 버튼을 클릭시 sendEmail의 값은 true로 바뀌게 된다.
+ 그러면 `v-if="sendEmail"` div 태그 안에 요소들이 보여지게 된다.
+ `Timer`는 component로 따로 분리되어 있다.
