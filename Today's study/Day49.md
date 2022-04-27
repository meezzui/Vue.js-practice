#### Count( > components) 
+ '+' 클릭 시 갯수 증가, '-' 클릭 시 갯수 감소
+ 결과 이미지

  ![image](https://user-images.githubusercontent.com/86812098/165415098-470d734a-3c96-4bc7-ac6f-07f288984939.png)
  
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
  watch: {
    countOption: {
      deep: true,
      handler(value) {
        this.dynamicCount = value.count
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.dynamicCount = this.countOption.count
      this.index = this.countOption.index ? this.countOption.index : 0
    })
  },
  methods: {
    handleSubtract() {
      if (this.dynamicCount > 0) {
        this.dynamicCount--
      }
      this.$emit('change', { count: this.dynamicCount, index: this.index })
      this.dynamicCount = this.countOption.count
    },
    handleAdd() {
      this.dynamicCount++
      this.$emit('change', { count: this.dynamicCount, index: this.index })
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

