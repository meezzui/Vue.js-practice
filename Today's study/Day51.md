#### 클래스 배열과 일반 클래스 사용😎
+ 클래스명도 배열로 묶어서 바인딩 할 수 있다.
+ `<ul :class="['event-contents', eventList.length === 1 ? 'center' : '']">`
  + 0번째 인덱스는 일반 클래스이고 1번째 인덱스는 삼항연산자를 이용하였다.
  + `eventList.length === 1 ? 'center' : ''` : `eventList`에 데이터가 한개만 들어있을 경우 'center'라는 클래스명의 css가 적용되게 하였다.

#### 클래스 배열과 일반 클래스의 차이
+ 배열을 사용하면 코드의 중복을 줄여준다.
+ `<ul :class="eventList.length  === 1 ?'event-contents center': 'event-contents' ">`
  + 배열을 사용하지 않을 경우 `event-contents`라는 클래스명을 true일 경우와 false일 경우 각각 써주어야 하기 때문에 코드 중복이 생긴다.
+ `<ul :class="['event-contents', eventList.length === 1 ? 'center' : '']">` 
  + 그러나 배열을 사용할 경우 한 번만 써주면 되기 때문에 코드 중복을 줄일 수 있다.
