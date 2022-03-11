### 오늘의 일기
---
#### @mixin
+ 그룹단위의 스타일을 변수처럼 적용할 수 있다.
+ 즉 여러개의 스타일을 설정해두었다가 한번에 적용하는 것이 가능하다.
+ `@mixin`을 사용할 때는 `@include`를 사용하면 된다.
```node
//두줄까지만 보이고 그 이후엔 ...으로 표시

@mixin limitTwoLine {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

// 사용
.notice-title-box{ @include limitTwoLine()}
```
#### mixin에 조건문 사용하는 방법 (if, else)
+ mixin은 전달 받은 인자를 사용하여 조건문 `@if, @else`를 사용할 수 있다.
+ 예) 폰트 사이즈를 키우거나 줄이기
```node
mixin fontSize($size) {
  @if $size == 'small' {
    font-size: 10px;
  }
  @else if $size == 'large' {
    font-size: 14px;
  }
  @else {
    font-size: 12px;
  }
}

//사용
div {
  @include fontSize('small');
  @include fontSize;
  @include fontSize('large');
}
```
#### `@content` 블럭 사용하는 방법
+ 기존의 `@mixin` 내부의 스타일을 추가하거나 변경하려면 `@content`를 사용!!
+ `@content`를 사용하면 `@include` 내부에 선언된 블럭을 함께 사용할 수 있다.
```node
@mixin box-default {
  padding: 20px 30px;
  margin-bottom: 20px;
  @content;
}
--------------------------------------------------
div {
  @include box-default {
    padding: 40px 60px;
  }
}
```

#### @mixin에 인자 arguments를 사용하는 방법
+ 상황에 따라 다르게 주고 싶다면? 인자를 사용한다.
+ 예) box-default의 padding 값을 상황에 따라 다르게 주고 싶을 경우
```node
@mixin box-default($padding, $margin) {
  padding: $padding;
  margin-bottom: $margin;
}
------------------------------------------------
div {
  @include box-default(20px, 30px);
}
```
+ div 태그는 padding과 margin-bottom 값이 각각 20px, 30px으로 선언될 것이다.
+ `@mixin`에 인자를 사용하여 선언해두면 코드의 재사용성을 높일 수 있다.

#### 기본값 선언하여 사용하기
+ 인자가 없는 경우 기본값을 설정할 수 있다.
+ 예) 아무 값도 입력되지 않은 경우 각각 10px, 20px로 선언
```node
@mixin box-default($padding: 10px, $margin: 20px) {
  padding: $padding;
  margin-bottom: $margin;
}
```
+ 값이 없어도 기본값이 적용되어 나타난다.





