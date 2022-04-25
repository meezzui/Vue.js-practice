#### nextTick()
+ Javascript는 비동기로 처기되기때문에 DOM의 모든 변경사항이 반영 되기 전에 DOM에 접근하려고하면 `undefined`나 `null` 에러가 발생한다.
+ 따라서 Vue.js에서는 데이터 갱신, UI 업데이트 등DOM의 모든 변경사항이 완전히 반영된 후에 로직을 실행할 수 있도록 `nextTick()`을 제공한다.
+ 다시말해, 렌더링 사이클 이후 실행될 콜백 함수를 등록할 수 있는 기능을 제공하는 메서드이다.
+ `$nextTick`의 콜백 함수 안에서 DOM을 조작하면, 데이터를 가지고 화면을 그리기 전에 DOM이 먼저 생성되며 원하는 트리(태그)를 찾지 못하는 오류를 막을 수 있다.😁
+ `$nextTick`은 `await/async`와도 함께 사용 가능하다.

```node
// 실행할 함수를 $nextTick()으로 감싸준다.
mounted() {
	this.$nextTick(function() {
		...함수실행
	})
    
	// ES6 경우
	this.$nextTick(() => {
		...함수실행
	})
}
```
+ 추가로..😏
  + `$forceUpdate`
    + 인스턴스를 강제로 다시 렌더링시킨다. 
    + 다른 컴포넌트나 인스터스에 영향을 끼치지 않고 해당 메서드가 실행된 인스턴스만 다시 렌더링된다.  
    + 언제 사용해??🧐 Vue의 상태는 변경되었으나 화면에 변경된 상태가 반영되지 않는 경우 이 메서드를 사용하며 다시 렌더링하며 반영시킬 수 있다. 
    + 하지만❗❗❗ 렌더링은 작업 자체가 비용이 많이 들기 때문에 과도하게 사용할 시 애플리케이션의 성능이 하락할 수 있다.

#### setTimeout()
+ 일정 시간 후에 특정 코드, 함수를 의도적으로 지연한 뒤 실행하고 싶을 때 사용하는 함수이다.
+ 즉, 콜백함수로 지연시간 뒤에 실행될 코드를 설정한다.
+ 사용법 -> `setTimeout(function() { // 지연시간 뒤에 실행될 코드 }, 딜레이시간);`
+ 언제 사용하는가?🧐
  + 접속 후 몇 초 후에 팝업 또는 배너창 띄우기
  + 방문자의 스크롤이 브라우저 일정 위치에 올 경우 몇 초 뒤에 애니메이션 실행
  + 검색창 또는 일부 섹션 몇 초 뒤에 사라질 경우
  + 방문자 접속 후 20-30초가 지난 뒤 메일 구독을 신청하는 팝업창을 띄울 경우
  + 버튼 클릭시 아래에서 올라오는 모달창 띄우기
+ 주의❗❗
  + `setTimeout()` 내에서는 `this.`을 사용하지 못한다.
  + why?😯 
  + javascript 에서 `this` 키워드는 기본적으로 전역 객체(window)를 this 바인딩한다.
  + 그래서 `this.`변수에 값을 할당하면 전역(window) 객체에 할당이 되어 에러가 뜬다.
  + 하나 더!! 화살표 함수 안에서도 `this.`을 사용할 수 없다.


#### `nextTick`과 `setTimout` 차이점✔
+ 공통점 : 둘다 콜백함수이다.
+ 차이점
  + `setTimeout` : 콜백함수가 Task Queue의 제일 끝으로 들어감
  + `nextTick` : 콜백함수가 Task Queue의 제일 앞으로 들어가서, 가능한 제일 빨리 실행될 수 있도록 함

#### `nextTick`과 `setTimout` 예제(버튼 클릭시 모달창 띄우기)
```node
watch: {
  modalState(value) {
    if (value) {
      this.$nextTick(_ => {
        setTimeout(_ => {
          document.getElementById('modal-wrapper').style.height = this.modalHeight
        }, 100)
      })
    } else {
      this.$nextTick(_ => {
        document.getElementById('modal-wrapper').style.height = 0
      })
    }
  },
  modalHeight(value) {
    this.$nextTick(_ => {
      setTimeout(_ => {
        document.getElementById('modal-wrapper').style.height = value
      }, 100)
    })
  }
},
props: ['modalState', 'modalHeight'],
methods: {
  onClickOutside() {
    this.$emit('close')
  }
}
```









