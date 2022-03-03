### 오늘의 일기
---
#### Computed
+ Computed를 사용하는 이유
  + 템플릿 태그 안에 많은 연산을 해야할 경우 템플릿이 복잡해진다.(Computed를 사용하면 코드가 간결해짐)
  + Getter의 기능을 사용할 수 있다.
+ Computed의 속성
  + 데이터 바인딩이 가능하여 data에 접근 가능하다.
  + data가 바뀜에 따라 computed값도 자동으로 바뀌어 렌더 된다.
  ```node
  <template>
    <div>
      <p>{{ ticketList }}</p>
      <p>{{ increse }}</p>
    </div>
  </template>
  <script>
  export default {
    data() {
      return {
        score: 1
      };
    },
    computed: {
      increse() {
        return score + 1;
        //data에 직접 접근하여 계산한 결과를 템플릿에서 보이게 한다.
      },
      ticketList() {
        return this.$store.getters["ticket/ticket"];
        // ticket.js라는 store에 접근해 ticket state를 가져온 후 템플릿에서 보이게 한다.
      }
    }
  };
  </script>
  ```
+ computed에 선언 가능하고 methods에도 선언 가능하다.
  ```node
  <template>
    <div>
      <p>{{ increse() }}</p>
    </div>
  </template>
  <script>
  export default {
    data() {
      return {
        score: 1
      };
    },
    methods: {
      increse() {
        return this.score++;
      }
    }
  };
  </script>
  ```
+ `computed`와 `methods`의 차이점
  + `computed`
    + 의존하는 값이 변하는 경우 계속 실행된다. (위의 예제에서는 score)
    + computed 속성은 종속 대상을 따라 저장(캐싱)된다는 것이다. 해당 속성이 종속된 대상이 변경될 때만 함수를 실행한다.
    + 즉, data안에 잇는 값이 변경되지 않는 한, computed 속성을 여러 번 요청해도 계산을 다시 하지 않고 계산되어 있던 결과를 즉시 반환한다.
  + `methods`
    + 랜더링 될 때만 함수가 실행된다.
    + 즉, 이 값이 랜더링 된 값으로 유지하고 싶다면 `methods`에 정의하면 된다.

+ Computed의 기능
  + 캐싱
    + 캐싱이란 반복된 연산 중 한 번만 로직이 실행되고 똑같은 값을 반환하는 것이다.
    + 데이터를 최적화하는 용도로 자주 사용된다.
    + computed를 통해 만든 계산된 데이터는 캐싱이라는 기능을 통해서 한 번 연산해놓은 값을 반복적으로 출력할 때 다시 한번 연산하지 않는다.
    + 왜냐하면 저장되어 있는 캐싱된 값으로 그대로 해당하는 내용을 화면에 출력하기 때문이다.
    ```node
    <template>
      <h1>{{ reversedMessage }}</h1> // 첫 출력 시 연산 작업 실행
      <h1>{{ reversedMessage }}</h1> // 실행 X
      <h1>{{ reversedMessage }}</h1> // 실행 X
      <h1>{{ reversedMessage }}</h1> // 실행 X
    </template>
    <script>
    export default {
      data() {
        return {
          msg: 'Hello Computed'
        }
      },
      computed: {
        reversedMessage() {
          return this.msg.split('').reverse().join('')
        }
      }
    }
    </script>
    ```
  + Getter/Setter
    + Getter란?
      + 값을 가져오는 것으로 값을 변경할 수 없으며 오직 값을 읽는 것만 가능하다.
    + Setter란?
      + 값을 변경 또는 저장하는 기능을 한다.
    + getter/setter 예시
    ```node
    <template> 
      <button @click="add">ADD</button> 
      <h1>{{ computedMessage }}</h1> 
      <h1>{{ computedMessage }}</h1> 
      <h1>{{ computedMessage }}</h1> 
      <h1>{{ computedMessage }}</h1> 
    </template> 
    <script> 
      export default 
        { 
          data() { 
            return { 
              // Getter, Setter msg: "Hello Computed!" 
            } 
          }, 
          computed: { 
            computedMessage: { 
              // 메소드 형식이 아닌 객체 형식으로 등록한다. 
              // 왜? getter와 setter를 내부에 정의해야 하기 때문이다. 
                get() { 
                  // getter 역할 return this.msg.split('').reverse().join('') }, 
                set(newValue) { 
                  // setter 역할, 매개변수를 받아서 값 재설정 this.msg = newValue } 
             } 
          }, 
          methods: { 
            add() { 
              this.computedMessage += '?!' // 기존 값에 추가되는 문자열(= '?!')이 setter의 매개변수(= newValue)에 들어감 
                                          // ex) computedMessage += '?!' 이 값이 newValue로 넘어가는 것 
            } 
          } 
      } 
    </script> 
    <style> 
      h1{ font-size: 50px; } 
    </style>
    ```

+ computed 속성 주의사항
  + 컴퓨티드 속성은 인자를 받지 않는다. 즉, 괄호를 붙여서는 안 된다.
    + `<div>{{ reverseMessage(false)}}</div>` 이렇게 하면 안 됨.
    + 템플릿에 선언한 컴퓨티드 속성에 괄호가 생기는 순간 해당 템플릿을 실제 DOM으로 변환할 때 라이브러리에서 에러를 발생시킨다.
    + `<div>{{reverseMessage}}</div>` 이렇게 써야 함.
  + HTTP 통신과 같이 컴퓨팅 리소스가 많이 필요한 로직을 정의하지 않는다.
    ```node
    data: {
      message: ''
    },
    computed: {
      reverseMessage() {
        var vm = this;
        axios.get('/message').then(function(response) {
          vm.message = response.data;
        });
        return this.message.split('').reverse().join('');
      }
    }
    ```
    + 기본적으로 컴퓨티드 속성은 템플릿 코드의 가독성을 위한 기능이며 HTTP 통신과 같이 브라우저 리소스가 많이 할애되는 코드들은 watch나 methods에 넣는 것이 적합하다.

#### Watch
+ 기존에 Vue 인스턴스 내에 선언된 값의 변화를 감시하는 역할을 하기 때문에 Vue 인스턴스 내에 선언된 값을 그대로 다시 사용하게 된다.
+ 예시
```node
new Vue({
	el: "#app",
  data: {
    count: 3,
    text: '변경 전입니다.'
  },
  watch: {
    count: function (newVal, oldVal) {
      this.text = oldVal + '에서 ' + newVal + '로 변경되었습니다.'
    }
  }
})
```
+ count를 감시하여 count의 값이 변화할 때마다 watch안에 정의한 count라는 함수가 실행되는 것이다.
+ 변수는 newVal과 oldVal을 인자로 받고 있다.
+ 감시하고 있는 대상의 변경된 값과 이전 값을 인자로 받을 수 있다.












