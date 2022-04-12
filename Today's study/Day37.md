#### 리스트 관련 함수
+ `some()`
  + 배열 안의 어떤 요소라도 주어진 판별 함수를 통과하는지 테스트한다.(예- 체크가 되었는지)
  + 구조 -> `some(변수명 => 변수.해당기능)`
  + 예시(button 활성화)
  ```node
  computed: {
    buttonValidation() {
      return this.couponChoiceList.some(cpn => cpn.checked)
    }
  }
  ```
+ every
+ map
+ filter
+ find
