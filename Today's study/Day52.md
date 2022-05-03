#### Pagination
+ component로 따로 빼서 사용
+ 코드 설명
  + ✔✔✔✔
```node
<template>
  <div id="pagenation">
    <div v-if="pageList[0].page > 10" role="btn" class="prev_btn" @click="handlePaging(-1, curPage)"/>
    <div
      v-for="(page, p) in pageList"
      :key="p"
      role="btn"
      class="each_page"
      :class="curPage - pageGroupIndex === (p + 1) ? 'on' : ''"
      @click="handlePaging(p, page.page)">
      <p>{{ page.page }} </p>
    </div>
    <div v-if="totalAmt > amtPerPage * (pageList[pageList.length - 1].page)" role="btn" class="next_btn" @click="handlePaging(10, curPage)"/>
  </div>
</template>

<script>
export default {
  name: 'Pagenation',
  props: {
    totalAmt: {
      type: Number,
      default: 0
    },
    amtPerPage: {
      type: Number,
      default: 0
    },
    curPage: {
      type: Number,
      default: 1
    }
  },
  data() {
    return {
      pageGroupIndex: 0
    }
  },
  computed: {
    pageList() {
      const result = []
      for (let index = this.pageGroupIndex; index < Math.floor((this.totalAmt - 1) / this.amtPerPage + 1); index++) {
        if (index < this.pageGroupIndex + 10) {
          result.push({ page: index + 1 })
        }
      }
      return result
    }
  },
  created() {
  },
  methods: {
    handlePaging(index, page) {
      let pageEmit = 0
      window.scroll(0, 0)
      if (index === 10) {
        this.pageGroupIndex += 10
        pageEmit = this.pageGroupIndex + 1
      } else if (index === -1) {
        this.pageGroupIndex -= 10
        pageEmit = this.pageGroupIndex + 1
      } else {
        pageEmit = page
      }
      this.$emit('change', pageEmit)
    }
  }
}
</script>

<style lang="scss" scoped>
*{
  font-size: 14px;
}
#pagenation{
      display: flex; justify-content: center; margin-top: 30px; align-items: center;
      .each_page{
        width: 26px;
        p {
           font-size: 14px; color: #BBBBBB; padding: 0 3px;  width: 20px; height: 20px; display: flex; align-items: center; justify-content: center;
        }
      }
      .on {
        p {
           color: white; background: $primary; border-radius: 50%;
        }
      }
      .next_btn {
        width: 26px; height: 26px;
        @include background('~@/assets/image/next.png', 26px 26px, 100% 50%);
      }
      .prev_btn {
        width: 26px; height: 26px;
        @include background('~@/assets/image/prev.png', 26px 26px, 100% 50%);
      }
}
</style>
```
