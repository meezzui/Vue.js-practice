### 오늘의 일기
---
#### 탭 메뉴
+ 둘 중하나 메뉴 선택하면 그에 해당하는 컴포넌트가 나온다.
+ `v-if`와 `v-else`이용
```node
<template #con>
  <div>
    <template v-if="isEvent">
     // 이 부분이 선택될 시 보여질 부분(html)
    </template>
    <template v-else>
      // 이 부분이 선택될 시 보여질 부분(html)
    </template>
  </div>
</template>

<script>
export default defineComponent({
  name: 'NoticeEventListPage',
  components: {
    IonPage
  },
  data() {
    return {
      isEvent: true
    }
  },
})
</script>
```
+ `isEvent`의 기본값을 true로 지정해준다.
+ v-if부분은 true일 때 작동한다. 따라서 처음엔 첫번째 탬플릿 안에 있는 화면이 보여질 것이다. 
+ 그리고 다른 메뉴를 탭하면 자연스럽게 false로 바뀌면서 else 부부에 있는 화면이 보여진다.
