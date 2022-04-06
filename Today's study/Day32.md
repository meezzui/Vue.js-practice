#### 버튼 활성화
+ 체크 표시하면 버튼이 활성화되게 한다.
+ `:isValid="buttonValidation" `: isValid를 바인딩해주면 따로 components로 버튼을 뺴준 것 덕분에 비활성화 색깔로 변한다.
+ ` isCheck: false`: data()에 이렇게 boolean값을 준다.
+ `<ion-checkbox v-model="isCheck"></ion-checkbox>` : `isCheck`을 `v-model`에 넣어주면 `buttonValidation()` 함수에서 그 값을 retrun 해줌으로써 버튼이 활성화 된다.
```node
---------------------(생략)----------------------
<div class="check">
  <ion-checkbox v-model="isCheck"></ion-checkbox>
</div>
---------------------(생략)----------------------
<div>
  <BottomButton :single="`${addComma('3000')}원 결제하기`" :isValid="buttonValidation" isPadding="true"/>
</div>

<script>
import { IonPage, IonCheckbox } from '@ionic/vue'

export default {
  name: '',
  components: {
    IonPage, IonCheckbox
  },
  setup() {
  },
  data() {
    return {
      isCheck: false
    }
  },
  computed: {
    buttonValidation() {
      return this.isCheck
    }
  },
  methods: {
  }
}
</script>
```


