### 오늘의 일기
---
#### vue.js `vue/multi-word-component-names` 오류 잡기
+ vue.js 규칙상 항상 파일이름과 컴포넌트 이름은 항상 여러단어로 지어주어야 한다.
+ 예를들어, 'toDo' 이런식으로!!
+ 그러나 단일어로 쓰고 싶거나 써야할 일이 생길 수 있다. 이때 `vue/multi-word-component-names`오류를 잡아주는 방법이 있다.
+ 🎈 `vue/multi-word-component-names` 오류 없애는 방법 🎈 
  + `.eslintrc.js`파일에서 `'vue/multi-word-component-names': 'off'`를 추가해주면 된다.
  + 주의!!🧨 설정해주고 서버를 껐다가 켜야 반영이 된다.


#### 어떤 태그에 `margin-top` 속성을 줄 때 윗 부분이 같이 따라 내려오는 경우(버그다!!😤)
+ 윗 부분 태그에 `overflow:auto` 처리를 해주면 해결된다.
```node
<div class="coupon-container">
  <div class="coupon-innerbox"> </div>
</div>

<style lang="scss" scoped>
.coupon-container{ overflow: auto; }
.coupon-innerbox{ margin: 20px auto; } 
</style>
```

#### vue.js 라우터 설정하기 
+ common.js에 라우터 관련 메소드를 설정해두면 편리하다.
```node
methods: {
    routeTo(name, query, params) {
      const queryInMethod = query || null
      const paramsInMethod = params || null
      // console.log({ query, params, paramsInMethod })
      this.$router.push({ name, queryInMethod, params: paramsInMethod })
      menuController.close('app-menu')
    }
----------------------------------------------------------------------------
//사용하기 
`@click="메소드이름('index.js에서 path설정에 name')"` 이것을 해당 태그안에 넣어주면 된다.

//예)
<div class="event-img-box"  @click="routeTo('eventDetail')">
 <img src="@/assets/image/temp/eventImage.png" alt="" class="event-img">
</div>
```
+ main.js에 common.js 파일을 import해서 전역으로 만들어 놔줬기 때문에 페이지마다 따로 import해줄 필요가 없다.
