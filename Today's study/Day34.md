#### 달력 모달창 기능 구현
+ 달력 이미지를 클릭하면 달력 모달창이 뜨고 날짜를 선택하면 그 선택된 날짜가 날짜란에 보이게 된다.
+ `<div v-for="(tax, t) in taxList" :key="t" class="tax_product_choice">` : 구매날짜 선택하는 칸과 물품 구분 칸을 반복문을 돌려준 것이다.
+ 달력은 ionic의 `ion-datetime` API를 사용했다.
```node
<div class="date_time_box">
  <p>{{tax.date}}</p>
  <ion-button :id="`open-modal_${t}`">
    <img src="@/assets/image/temp/calender.png" alt="calender">
  </ion-button>
  <ion-modal :trigger="`open-modal_${t}`">
      <ion-datetime v-model="tax.date" presentation="date" @ionFocus="onDateChanged"></ion-datetime>
  </ion-modal>
</div>
```
+ `<p>{{tax.date}}</p>`: 선택한 날짜의 데이터가 들어오는 부분이다.
+ `<ion-button :id="`open-modal_${t}`">`, `<ion-modal :trigger="`open-modal_${t}`">`
  + 버튼의 id값과 modal창의 trigger값을 같게 하여 바인딩 해줌으로써 버튼을 눌렀을 때 모달창이 뜨게 한다.
  + 여기서 `${t}` 이렇게 해준 이유는 구매날짜 선택하는 칸과 물품 구분 칸이 추가 버튼을 누르면 똑같이 추가가 되는데 그 인덱스별로 다른 데이터 값이 들어오게 하기 위함이다.
+ `<ion-datetime v-model="tax.date" presentation="date" @ionFocus="onDateChanged"></ion-datetime>`
  + `v-model="tax.date"` 양방향 통신을 할 수 있게 한다.
  + `presentation="date"` : `ion-datetime` API의 하나로써 날짜를 선택할 수 있는 달력 모달창만 보여준다.(다른걸 더 추가할 수 있다)
  + `@ionFocus="onDateChanged"` : `onDateChanged`라는 함수를 선언하여 focus되었을 때 모달창이 사라지게 한다.

+ 모달창의 위치와 모양은 style에 `ion-modal`와 `ion-modal ion-datetime`로 지정해주었다.
```node
<template>
  <ion-page>
      <BaseLayout :header="{title: '사후 택스리펀 계산기', cart: true}" :useBottomMenu="true">
        <template #con>
        ---------------------(생략)-----------------------
          <div class="tax_detail">
            <div v-for="(tax, t) in taxList" :key="t" class="tax_product_choice">
              <div class="buy_date">
                <div class="buy_date_p">
                  <p>구매 날짜</p>
                </div>
                <div class="date_time_box">
                  <p>{{tax.date}}</p>
                  <ion-button :id="`open-modal_${t}`">
                    <img src="@/assets/image/temp/calender.png" alt="calender">
                  </ion-button>
                  <ion-modal :trigger="`open-modal_${t}`">
                      <ion-datetime v-model="tax.date" presentation="date" @ionFocus="onDateChanged"></ion-datetime>
                  </ion-modal>
                </div>
              </div>
              <div class="pro_kind">
                <div class="pro_kind_p">
                  <p>물품 구분</p>
                </div>
                <div class="sel_pro_kind">
                  <select v-model="tax.selected" name="kind" id="select_kind" :style="tax.selected === 'placeholder' ? 'color: #B2B2B2;' : ''">
                    <option value="placeholder" selected>구분 선택</option>
                    <option value="미확정">구분 항목 미확정</option>
                  </select>
                </div>
              </div>
            </div>
          </div>
          ---------------------(생략)-----------------------
        </template>
      </BaseLayout>
  </ion-page>
</template>

<script>
import { IonPage, IonButton, IonModal, IonDatetime, modalController } from '@ionic/vue'

export default {
  name: '',
  components: {
    IonPage, IonButton, IonModal, IonDatetime
  },
  setup() {
  },
  data() {
    return {
      blankTaxForm: { date: '', selected: 'placeholder' },
      taxList: []
    }
  },
  created() {
    this.taxList.push({ ...this.blankTaxForm })
  },
  methods: {
    addTaxObject() {
      this.taxList.push({ ...this.blankTaxForm })
    },
    deleteData(index) {
      this.taxList.splice(index, 1)
    },
    onDateChanged() {
      modalController.dismiss()
    }
  }
}
</script>

<style lang="scss" scoped>

ion-modal {
  --width: 290px;
  --height: 382px;
  --border-radius: 8px;
}

ion-modal ion-datetime {
  height: 382px;
}
---------------------(생략)-----------------------
</style>
```
