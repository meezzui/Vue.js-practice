#### `arr.reduce()` 함수
+ `reduce()`에는 누산기가 포함되어 있기 때문에 배열의 각 요소에 대해 함수를 실행하고 누적된 값을 출력할 때 용이하다.
+ `map()`, `fileter()`, `join()`, `sort()`, `every()`, `some()`, `find()`, `findIndex()`, `includes()` 등도 모두 `reduce()`로 실행이 가능하고, javascript에서 가장 강력한 기능 중 하나이다.
+ `reduce` 는 왼쪽 원소부터 콜백 함수를 실행한다.(*`reduceright`은 오른쪽 원소부터)
+ 형태 -> `arr.reduce(콜백함수, 초기값)`  
+ `reduce()` 함수는 어떨때 이용될까?🧐
  + 해당 배열의 총 합을 반환할 때
  + 누적으로 어떤 값을 넣어주거나 초기화 시킬 때
    + 예) `select박스 초기화` - 상위 옵션 값 변경시 선택한 하위 옵션들(옵션 목록) 초기화  
    ```node
    // 옵션 값 변경 시
    setOptions(idx) {
      // 옵션 선택 완료
      if (idx + 1 === this.optionSet.length) {
        this.options.some((opt) => {
          let targetYn = true
          for (let i = 0; i <= idx; i++) {
            const name = 'optVal' + (i + 1)

            if (opt[name] !== this.selectedOpt[i]) {
              targetYn = false
              break
            }
          }
          // 선택했던 selectBox 초기화
          // this.selOpt = []
          if (targetYn) {
            // 중복 체크
            const already = this.orderOpt.some(x => (x.optNo === opt.optNo))
            if (!already) {
              const result = {
                optNo: opt.optNo,
                cnt: 1,
                optNm: ''
              }
              const optNm = []
              for (let i = 1; i <= 3; i++) {
                const optVal = 'optVal' + i
                if (opt[optVal] !== '') {
                  optNm.push(opt[optVal])
                }
              }
              result.optNm = optNm.join('/')
              this.orderOpt.push(result)
              // 최종 금액
              this.setTotalPrice()
            } else {
              this.$alert({
                msg: '이미 선택된 옵션입니다.',
                middle: '확인'
              })
            }
            this.setDefaultOption()
            return true
          }
        }, {})
      } else {
        // 상위 옵션 값 변경 시 선택한 하위 옵션들 초기화
        console.log(idx)
        if (this.selectedOpt.length > 1) {
          this.selectedOpt = this.selectedOpt.reduce((temp, select, selectIdx) => {
            if (selectIdx > idx) {
              select = ''
            }
            temp.push(select)
            return temp
          }, [])
          // 상위 옵션 값 변경 시 하위 옵션 목록 초기화
          this.optionSet = this.optionSet.reduce((temp, opt, optIndex) => {
            if (optIndex > idx) {
              opt.values = []
            }
            temp.push(opt)
            return temp
          }, [])
        }
        // 다음 옵션 값 추가
        this.optionSet[idx + 1].values = this.options.reduce((temp, opt) => {
          // 선택한 상위 옵션 값에 맞는 옵션인지 체크
          let optionYn = true

          for (let i = 0; i <= idx; i++) {
            const optVal = 'optVal' + (i + 1)

            if (opt[optVal] !== this.selectedOpt[i]) {
              optionYn = false
              break
            }
          }

          // 선택한 상위 옵션 값에 맞는 옵션이면 다음 옵션 데이터에 추가
          if (optionYn) {
            temp.push(opt['optVal' + (idx + 2)])
          }

          return [...new Set(temp)]
        }, [])
      }
      console.log(this.selectedOpt)
    }
    ```

🎈 초기값은 최초의 리듀서 호출에서 `accumulator(누산기)`에 제공하는 값이다. 초기값이 없다면 배열의 첫번째 요소(0번 인덱스)를 사용하고 초기값이 있다면 주어진 초기값을 사용한다.  


💜초기값이 없을 경우💜  
+ 배열의 첫번째 요소(0번 인덱스)를 누산기에 누적한 후 1번 인덱스부터 `reducer 함수`를 거친다.
+ 배열의 요소 값이 1개인데 초기값도 제공하지 않은 경우 or 초기값은 있지만 배열이 빈 배열인 경우에는 그 단독 값을 리듀서를 거치지 않고 바로 반환한다.
+ 배열이 비었는데 초기값도 없는 상태에서 `reduce()`를 호출하면 `TypeError` 오류가 발생한다.
+ 즉, 초기값을 주어야 한다❗❗

#### 참고📢 Filter 함수
+ `Reduce 함수`를 얘기할 때, map 함수만큼이나 자주 비교되는 것이 Filter 함수이다.
+ `Filter 함수`는 배열 각 요소에 대하여 주어진 함수의 결과값이 `true`인 요소들만을 모아서 새로운 배열을 반환하는 함수이다.
+ map 함수와 filter함수의 차이점✔
  + `map`의 콜백 함수의 리턴 값이 `string, number, array` 등이 다양했던 것에 비해 `filter`의 콜백 함수의 리턴 값은 오직 `boolean`이다.
+ filter 함수 VS reduce 함수
```node
// filter
const Number = [1, 2, 3]
let arr = Number.filter((v) => (v % 2 === 0))
arr;	// [2]

=================================================
// reduce- 초기값에 빈 배열을 주어서 배열을 반환할 수 있게 하였다.
const Number = [1, 2, 3]
let arr = Number.reduce((acc, cur) => {
    if (cur % 2 === 0) {
        acc.push(cur);
    }
    return acc;
}, []);
arr;  // [2]
```
