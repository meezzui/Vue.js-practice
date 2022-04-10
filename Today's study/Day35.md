#### 똑같은 틀의 요소 추가
+ 추가 버튼을 누르면 해당 요소의 object가 배열에 추가되면서 똑같은 형태의 틀이 복사됨
+ `<div v-for="(tax, t) in taxList" :key="t" class="tax_product_choice">` : 복사될 요소를 반복문을 돌린다. 그리고 data()에 리스트는 빈 배열로 둔다. -> `taxList: []`
+ `blankTaxForm: { date: '', selected: 'placeholder' }`: object 형태를 선언해준다.
+ `<div class="detail_plus_box" @click="addTaxObject">`
  + 추가 버튼을 누르면 `addTaxObject` 함수가 실행된다.
  ```node
  addTaxObject() {
     this.taxList.push({ ...this.blankTaxForm }) // `...`을 통해서 object형태 그대로 배열에 복사되게 한다.
   }
  ```
+ `<div v-if="taxList.length > 1" class="del_btn">`
  + 처음 페이지는 `물품구분/구매날짜/물품가격` 틀이 하나만 보이고 삭제버튼은 추가버튼을 누른 후에 나타나게 한다. 
  + 리스트의 인덱스가 1보다 클 경우에만 삭제버튼이 나타나게 한다.
+ `<p @click="deleteData(t)">삭제</p>`
  + 삭제 버튼을 누르면 `deleteData()` 함수가 실행된다.
  ```node
   deleteData(index) { // 함수 실행할 때 인덱스를 받아옴 -> 이름은 내가 짓고싶은 걸로 해도 됨
      this.taxList.splice(index, 1) //splice() 함수를 써서 배열의 인덱스 하나를 삭제되게 한다. 
    }
  ```
  + `deleteData(t)` : 함수에 `t` 인덱스를 넘겨준다. 그러면 함수가 실행될 때 받은 해당 인덱스의 값이 적용되어서 내가 삭제하기 원하는 요소의 object가 삭제된다.

#### select테그 placeholder 색깔 바꾸기
+ 아무것도 선택하지 않았을 경우 defalut값의 글자 색깔은 `color: #B2B2B2`이렇게 나오고 선택했을 경우의 글자 색은 `color:black`이 되게 하기 위함이다.
+ 먼저 style부분에서 select태그의 기본 색깔을 `color:black(color: $bk;)`으로 둔다.
+ `<select v-model="tax.selected" name="kind" id="select_kind" :style="tax.selected === 'placeholder' ? 'color: #B2B2B2;' : ''">`
  + `v-model="tax.selected"` : 양방향 바인딩을 해준다.
  + `:style="tax.selected === 'placeholder' ? 'color: #B2B2B2;' : ''"` : 선택된 것의 value값이 `placeholder`이면 글자 색깔이 `color: #B2B2B2;` 이렇게 나오게 하고 아니면 style에 설정해둔 black이 적용되게 한다.
+ `<option value="placeholder" selected>구분 선택</option>` : 그리고 option태그에 젤 처음에 선택되게 할 부분(color: #B2B2B2 글자색)의 value 값에 `placeholder`라고 적어준다.
```node
<template>
  <ion-page>
      <BaseLayout :header="{title: '사후 택스리펀 계산기', cart: true}" :useBottomMenu="true">
        <template #con>
          <div id="tax_contain">
            <div class="tax_header">
              <div class="tax_header_p">
                <p>택스리펀 대상 물품 정보</p>
              </div>
              <div class="detail_plus_box" @click="addTaxObject">
                <div class="plus_img">
                  <img src="@/assets/image/temp/plus.png" alt="plus_tax_product">
                </div>
                <div class="plus_p">
                  <p>추가</p>
                </div>
              </div>
            </div>
            <div class="tax_detail_box">
              <form>
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
                    <div class="pro_price">
                      <div class="pro_price_p">
                        <p>물품 가격</p>
                      </div>
                      <div class="input_price">
                        <input type="number" placeholder="가격 입력">
                      </div>
                      <div class="tax_pro_price_symbol">
                        <p>원</p>
                      </div>
                    </div>
                    <div v-if="taxList.length > 1" class="del_btn">
                      <p @click="deleteData(t)">삭제</p>
                    </div>
                  </div>
                </div>
----------------------------(생략)-------------------------------
              </form>
            </div>
          </div>
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
------------------(생략)---------------------
#tax_contain{
    .sel_pro_kind{
      select{
        width: 250px;
        height: 42px;
        padding: 10px 35px 10px 15px;
        outline: none;
        font-size: 14px;
        color: $bk;
        border-radius: 6px;
        appearance: none;
        border: none;
        @include background('@/assets/image/temp/down-arrow.png', 12.76px 6.38px, 95% 50%, #F5F5F6);
      }
    }
  }  
}
------------------(생략)---------------------
</style>
```
