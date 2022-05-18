#### select 박스 선택 후 아래 div 보이기
+ select박스를 클릭하고 쿠폰을 선택하면 아래 상품가격,할인가격 등이 보여진다.
+ 주의❗❗❗ `@click` 이벤트를 사용하면 안 된다. => 이렇게 하면 select박스를 클릭함과 동시에 잠깐 보여졌다가 사라진다.
+ `watch: {}` 생명주기를 이용한다.😎 
+ `watch 함수`는 데이터가 변경되었을 때 타는 함수이다. 즉, select박스의 값이 변경되면 이 함수를 타게 된다.
```node
data() {
  return {
    couponChoiceList: [{}, {}, {}, {}],
    copiedCouponChoiceList: [],
    selectedCpn: [],
  }
}
watch: {
  couponChoiceList: {
    deep: true, // 배열안에 object를 타기 위한 코드
    handler(array) { // handler 함수 사용. handler는 새로운 value와 기존의 value를 비교할 수 있는 함수이다.
      array.forEach((a, index) => { // 반복문으로 select의 value값을 비교해줌 
        if (a.selected !== 'placeholder') { // 선택된 것이 'placeholder'라는 value값이랑 다를 경우 if문을 탐
          if (a.selected !== this.copiedCouponChoiceList[index].selected) { // 
            a.clicked = true // clicked에 true 값을 넣어줌
          }
        } else { //선택된 것이 'placeholder'라는 value값이랑 같을 경우 else문을 탐
          a.clicked = false // clicked에 false 값을 넣어줌
        }
      })

      this.copiedCouponChoiceList = this.array // copiedCouponChoiceList에 기존 배열 복사함
    }
  }
}
```
#### 여기서 잘 생각해야 할 점❗❗❗
+ 위에 처럼 코드를 짜면 select박스를 클릭하고 쿠폰을 선택하면 아래 상품가격,할인가격 등이 보여지는 기능이 수행되지 않을 것이다.
+ `this.copiedCouponChoiceList = this.array` 이렇게 하면 계속 값이 `false`가 들어간다.
+ 콘솔을 찍어보니 새로운 value값이랑 기존의 value 값이 같게 들어가고 있었다.(그니까 계속 false로 나오지...😥)
+ 그러면 왜 값이 같게 들어가는 것일까??🧐
  + 배열 전체가 변경되기 때문이다.






