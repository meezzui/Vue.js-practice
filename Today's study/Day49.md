#### Count( > components) 
+ '+' 클릭 시 갯수 증가, '-' 클릭 시 갯수 감소
+ 결과 이미지

  ![image](https://user-images.githubusercontent.com/86812098/165415098-470d734a-3c96-4bc7-ac6f-07f288984939.png)
  
+ `<div role="button" class="subtract" @click="handleSubtract"/>` 
  + `role="button"`: 요소가 버튼이지만 버튼 기능을 제공하지 않는다요소가 버튼이지만 버튼 기능을 제공하지 않는다. 그러나 속성 button과 함께 사용 하여 토글 버튼을 생성 할 수 있다.
  + css로 형태를 잡아줘야 한다.
  + 별도의 키 이벤트 핸들러를 요소에 추가해야 한다. - `@click="handleSubtract"`
  + `@click="handleSubtract` : 버튼 클릭 시 `handleSubtract`함수 실행. 숫자가 0보다 클 경우에만 '-' 클릭 시 숫자가 내려감.
+ `<p :class="countOption.count > 0 ? '' : 'off'">{{ countOption.count }}</p>`: 숫자 counting 보여주는 부분. counting이 '0'인 경우는 숫자 색깔 회색!! 
+ `<div role="button" class="add" @click="handleAdd"/>` : counting 증가버튼. 버튼 클릭시 `handleAdd`함수 실행
```node
props: {
  countOption: {
    type: Object,
    default: () => {
      return { count: 0 }
    }
  }
```
+ `props`로 타입은 object, count는 0으로 보내줬다.
+ 코드
```node
<template>
  <section id="smallCount">
    <div role="button" class="subtract" @click="handleSubtract"/>
    <p :class="countOption.count > 0 ? '' : 'off'">{{ countOption.count }}</p>
    <div role="button" class="add" @click="handleAdd"/>
  </section>
</template>

<script>
export default {
  name: 'Count',
  props: {
    countOption: {
      type: Object,
      default: () => {
        return { count: 0 }
      }
    }
  },
  data() {
    return {
      dynamicCount: 0,
      index: 0
    }
  },
  watch: { //watch는 감시할 데이터를 지정하고 그 데이터가 바뀌면 어떠한 함수를 실행하라는 방식의 명령형 프로그래밍 방식
    countOption: {
      deep: true,
      handler(value) {
        this.dynamicCount = value.count
      }
    }
  },
  mounted() { //초기 렌더링 직전에 컴포넌트에 직접 접근
    this.$nextTick(() => {
      this.dynamicCount = this.countOption.count
      this.index = this.countOption.index ? this.countOption.index : 0
    })
  },
  methods: {
    handleSubtract() {
      if (this.dynamicCount > 0) { //숫자가 0보다 클 경우
        this.dynamicCount-- //내려주기
      }
      this.$emit('change', { count: this.dynamicCount, index: this.index }) //사용단에서 @change 함수를 써서 사용
      this.dynamicCount = this.countOption.count
    },
    handleAdd() { //숫자 증가시켜주기
      this.dynamicCount++
      this.$emit('change', { count: this.dynamicCount, index: this.index }) ////사용단에서 @change 함수를 써서 사용
      this.dynamicCount = this.countOption.count
    }
  }
}
</script>

<style lang="scss" scoped>
*{
  font-family: Roboto;
  font-size: 16px;
}
#smallCount {
  width: 76px; height: 34px; border: 1px solid #BBBBBB; border-radius: 5px; background: white; display: flex; justify-content: space-between; position: relative;box-sizing: border-box;
  padding: 0 10px; z-index: 500;
  div {
    height: inherit; width: 10px;
  }
  p {
    position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);font-size: 16px; color: $red; font-family: Roboto-Bold;
  }
  .off {
    color: $l-gray;
  }
  .subtract{
    @include background('~@/assets/image/small_sub.png', 10px 1px, 50% 50%);
  }
  .add {
    @include background('~@/assets/image/small_add.png', 10px 10px, 50% 50%);
  }
}
</style>

