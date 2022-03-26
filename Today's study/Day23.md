### 메뉴 하나씩만 선택되게 하기(선택 될 때마다 css변경 됨)
+ 하나씩만 선택되게 하려면 `input태그`에 타입을 `radio`로 해야 한다.
+ `type="radio"`로 주면 선택하는 기본형태의 아이콘이 나온다. 그런데 그것을 쓰지 않고 원하는 모양으로 custom하기 위해서 css에서 기본속성을 없애는 `all:unset`이라는 것을 input태그에 선언해 준다.
+ ` v-for="(order, i) in orderMenuList" :key="i"`: for문을 이용하여 메뉴 카테고리가 `orderMenuList`배열의 데이터 만큼 들어오게 한다.
+ `<label :for="order">{{order}}&nbsp;<span>3</span></label>`: label태그 안에 `{{order}}`를 해줌으로써 그 안에 데이터들이 들어온다.
+ 아래의 코드는 선택될 때마다 css가 변하도록 해주는 코드이다.
```
input:checked + label {
      background-color: #22BDB6; color: white; border: none;
      span{
        color: white;
      }
    }
```
+ 그런데 처음에는 위의 코드가 하나의 카테고리에만 적용되고 나머지는 적용이 되지 않았다.(input태그에 `id값`과 label태그에 `for값`을 똑같게 주었는데도 안됨😥)
```
//처음 코드
<div v-for="(order, i) in orderMenuList" :key="i">
  <input type="radio" id="order" name="orderCate"  :value="order">
  <label for="order">{{order}}&nbsp;<span>3</span></label>
</div>

// 바꾼 코드
<div v-for="(order, i) in orderMenuList" :key="i">
  <input type="radio" :id="order" name="orderCate"  :value="order">
  <label :for="order">{{order}}&nbsp;<span>3</span></label>
</div>
```
+ input태그의 id와 label의 for를 바인딩해주니 잘 되었다!!!

```node
<template>
  <ion-page>
    <BaseLayout :header="{title:title, back:true}" background="#F5F5F6">
      <template #con>
        <div>
          <div class="order-mini-header-contain">
            <div v-for="(order, i) in orderMenuList" :key="i">
              <input type="radio" :id="order" name="orderCate"  :value="order">
              <label :for="order">{{order}}&nbsp;<span>3</span></label>
            </div>
          </div>
          <div class="order-list-contain">
            <div class="order-list-box">
              <div v-for="(lists, i) in orderList" :key="i" class="order-list-contents">
                <div class="order-innerbox">
                  <div class="order-list-innerbox1">
                    <div>
                      <span>2021.11.12&nbsp;(20123453123)</span>
                    </div>
                    <div :class="isValue ? '': 'off'">
                      <span>결제완료</span>
                    </div>
                  </div>
                  <div class="order-list-innerbox2">
                    <span>[닥스] 데일리 정장구두</span>
                  </div>
                  <div class="order-list-innerbox3">
                    <span>17,800원</span>
                  </div>
                </div>
                <div class="go-to-detail">
                  <p>상세보기</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss" scoped>
.order-mini-header-contain{
  position: fixed;
  display: flex;
  padding: 9px 15px;
  background: white;
  width: 100%;
  div{
    &:nth-child(2){
      margin: 0 7px 0 7px;
    }
    input{
      all: unset;
    }
    input:checked + label {
      background-color: #22BDB6; color: white; border: none;
      span{
        color: white;
      }
    }
    label{
      font-size: 14px;
      color: #717377;
      border: 1px solid #EBEBEB;
      border-radius: 16px;
      background-color: #FFFFFF;
      padding: 5px 12px;
      span{
        font-size: 14px;
        color: #EF3F3E;
      }
    }
  }
}
------------------------(생략)------------------------
</style>

<script>
import { IonPage } from '@ionic/vue'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'OrderMainPage',
  components: {
    IonPage
  },
  data() {
    return {
      title: '구매 내역',
      orderMenuList: ['전체', '결제완료', '취소/반품'],
      orderList: [1, 1, 1, 1, 1, 1],
      isValue: true
    }
  },
  created() {
  },
  methods: {
  }
})
</script>
```











