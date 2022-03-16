### 오늘의 일기
---
#### `flex-grow`사용하기
<img src="https://user-images.githubusercontent.com/86812098/158303290-3fd97cb9-aa7b-4884-a8dc-f341ff1d6edb.png"/>

+ 위처럼 두개가 분활되게 하여 그 위에 요소들을 배치해야 할 경우 한쪽 요소에 `width` 값을 주고 나머지 한쪽에 `flex-grow:1`을 주면 자동으로 나머지 크기에 맞춰진다.
+ `flex-grow:1`
  + flex-grow 값을 1로 지정하면 사용가능한 공간은 각 항목에게 동일하게 분배되며, 각 항목은 주축을 따라 분배받은 값만큼 사이즈를 늘려 공간을 차지한다.
+ 첫 항목 `flex-grow:2`로 지정하고 나머지 두 개의 항목을 `flex-grow:1`로 지정
  + 각 항목에 지정된 `flex-grow` 값의 비율에 따라 남은 공간이 분배된다.
  + 각 항목의 `flex-grow` 비율이 `2:1:1` 이므로 첫 항목에게 100 픽셀, 두 번째와 세 번째 항목에게 50 픽셀씩 분배된다.

#### header 고정 스크롤
+ `position: fixed;`로 먼저 `header`를 고정하고 `background`를 `header`와 같은 색깔을 주고 `top`으로 위치를 잡아준다.
+ 그리고 그 아래 `contents`부분은 데이터가 많아질 경우 스크롤이 생기는데 그 높이를 계산해서 지정해주어야 한다. 
+ 높이는 `height: calc(높이 계산);` 를 사용한다. 모바일의 기본 높이는 `100vh`이다. 그러므로 `100vh- (상단메뉴의 높이)`를 빼주면 된다.
+ 그리고 `margin-top`과 `padding-bottom`으로 위 아래 간격을 조정해주면 된다.
```node
<template>
<div class="ticket-header">
  <div class="ticket-unuse">
    <button :class="isTicket ? 'ticket-unuse-tab on' : 'off'" @click="clickedButton(true)">미사용</button>
  </div>
  <div class="ticket-use">
    <button :class="!isTicket ? 'ticket-use-tab on':'off'"  @click="clickedButton(false)">완료/만료</button>
  </div>
</div>
<div class="ticket-contain">
  <div class="ticket-contents-box box-margin" v-for="(items,i) in [1,1,1,1,1,1,1]" :key="i" :class="isTicket ? '' : 'expire'" @click="routeTo('voucherDetail')">
  </div>
</div>
</template>

<style lang="scss" scoped>
.ticket-header{width: 360px; height: 38px; position: relative; border-bottom: solid 1.5px #EBEBEB; position: fixed; background: white;top: 56px;}
.ticket-contain{background-color: #F5F5F6; height: 100%; height: calc(100vh - 94px); margin-top: 38px; overflow: auto; padding-bottom: 100px;}
</style>
```




