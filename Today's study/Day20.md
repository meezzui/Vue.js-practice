### 컴포넌트 간의 통신
+ 기본적으로 `부모->자식`으로의 데이터 전달은 `props`를 통해, `자식->부모`로의 데이터 전달은 `이벤트 발생(emit)`을 통해 이루어진다. 
+ `양방향 바인딩(.sync)`을 2.3.0 이상 버전에서 제공하고 있으나, 웬만한 경우에는 사용을 지양하는 것을 권하고 있다. 
+ 자식 컴포넌트가 부모 컴포넌트 요소를 임의로 변경할 경우, 그 요소는 어디서 변경된건지를 알 수 없기 때문이다.


### 카테고리 슬라이드
+ `swiper` 태그 사용(swiper 옵션을 사용할 수 있음)
  + `:slidesPerView="'auto'"` : 글씨 길이에 맞춰서 해당 보더가 자동으로 늘어나고 줄어듦. 그래서 글씨길이가 길더라도 해당 보더가 아래로 넘치지 않음
  + `:spaceBetween="10"` : 카테고리 간의 간격
+ `v-model="selectedCategory"` : 입력 폼 바인딩으로 `v-model`은 `v-bind`와 `v-on`의 기능의 조합으로 동작한다. `부모->자식` 컴포넌트 태그에 v-model을 사용하면, 해당 data가 자식의 props로 넘어가고 자식에서 이벤트를 발생시켰을 경우 넘긴 데이터가 변수에 할당된다.
+ ` type="radio"`: 중요!!✨ 카테고리가 하나씩 선택되게 하기 위해선 `input 태그`의 `type`을 `radio`로 해줘야 한다.(기본적으로 설정된 radio 이미지를 없애기 위해선 `all:unset`을 해준다.)
+ style 부분은 카테고리가 체크되었을 때와 안 되었을 때 배경색깔과 글씨 색깔을 변경해주는 css이다. 
+ `v-for="(category, c) in list"` : 'list'라는 이름의 리스트 안에 데이터만큼 카테고리를 보여준다. 
+ `props: ['list']`: props를 사용하여 리스트를 하위 컴포넌트에 전달해준다.
```node
//상위 컴포넌트(이 부분만 따로 컴포넌트로 빼줌)
<template>
  <swiper :slidesPerView="'auto'" :spaceBetween="10" class="swiper">
    <swiper-slide v-for="(category, c) in list" :key="c" class="swiper-slide">
        <input v-model="selectedCategory" :id="category.value" name="favoriteCategory" type="radio" :value="category" :checked="c === 0">
        <label :for="category.value" @click="$emit('select-category', category.key, category.value)">{{category.value}}</label>
    </swiper-slide>
  </swiper>
</template>

<style lang="scss" scoped>

  .swiper{
    width: calc(100% - 30px); margin: 0 auto;
    .swiper-slide {
      width: max-content;
      input {
        all: unset;
      }
      input + label{
        font-size: 14px; display: inline-block;  color: #717377;  padding: 5px 13px; height: 32px; width: max-content;
        border-radius: 16px; border: 1px solid #EBEBEB;
      }
      input:checked + label {
        background-color: #22BDB6; color: white; border: none;
      }
    }
  }

</style>

<script>
import {
} from '@ionic/vue'
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'CategorySlideComponent',
  components: {
  },
  props: ['list'],
  setup() {
    return {

    }
  },
  data() {
    return {
    }
  }-
})
</script>

// 하위 컴포넌트(실제 보여지는 컴포넌트)
<template #con>
<section id="swiper-section" class="swiper-section">
  <CategoryMenu :list="categoryList" @select-category="selectCategory"/>
</section>
</template>

<script>
import CategoryMenu from '@/components/CategorySlide.vue'

export default defineComponent({
  name: 'FavoriteListPage',
  components: {
    IonPage, Product, CategoryMenu
  },
  props: ['list'],
  setup() {
    return {

    }
  },
  data() {
    return {
      selectedCategory: '',
      categoryList: [{ key: 'ENTIRE', value: '전체' }, { key: 'FOOD', value: '음식' }, { key: 'COFFEE', value: '카페/디저트' }]
    }
  }
})
</script>

```
