### 오늘의 일기
---
#### 이벤트 처리
+ `v-on` 디렉티브를 이용한다.
+ 줄여서 `@`로 대체할 수 있다.
+ `<button id="goodboy" @click="good">좋아요</button>` : 좋아요 버튼을 클릭하면 <script>안의 good() 메서드가 호출된다.
+ click 이벤트
  + `<button @click="handler('Hello')">click!</button>` : 클릭했을 시 `Hello`가 화면에 나타남.
  
#### 메소드 이벤트 핸들링
+ 로직을 `@click`에 바로 넣는 것이 아니라 메소드를 만들어 핸들링하는 것이 많은 양의 데이터를 처리하기에 용이하다.
+ 인라인 메소드 헨들러
  + handler에 들어가는 인수(여기서는 event)는 클릭 이벤트가 발생했을 때 정보를 갖는 객체(mouse event)로 이곳에서 필요한 속성을 가져와 사용할 수 있다. 
  + 인수는 매개변수이기 때문에 다른 이름으로 변경이 가능하다.
  ```node
  <button @click="handler">
    click!!
  </button>
  methods: {
    handler(event) {
      console.log(event)
      console.log(event.target) // <button>click!!</button>
      console.log(event.target.textContent) // click!!
    }
  }
  ```
#### 인라인 이벤트 핸들러 + 이벤트 헨들러 
+ 인라인 이벤트 핸들러(인수사용)에 이벤트 핸들러를 함께 사용하는 방법
+ `$`를 붙인다.
+ `<button @click="handler('hello', $event)">click!!</button>`
  
#### 다중 이벤트 핸들러
+ 인수가 없어도 호출하겠다는 의미로 소괄호()를 명시해야 한다.
```node
<button @click="handlerA(), handlerB()">click!!</button>
methods: {
  handlerA(){로직},
  handlerB(){로직}
}
```
  
#### 이벤트 수식어
+ 이벤트명에 `.`으로 연결하여 사용
+ 기본 수식어
  + `.prevent`: HTML의 기본 동작을 방지하고 method만 실행하는 기능이다.(preventDefault() 과 동일한 기능)
  + `.stop`: 이벤트가 전파되는 것을 중단 시킴(Bubbling 중단, stopPropagation() 과 동일한 기능) 
  + `.capture`: 포착 단계에서만 이벤트를 발생시킴(내부 엘리먼트를 대상으로 하는 이벤트가 해당 엘리먼트에서 처리되기 전에 여기서 처리함) 
  + `.self`: 발생 단계에서만 이벤트를 발생시킴 
  + `event.target`이 엘리먼트 자체인 경우에만 트리거를 처리, 자식 엘리먼트에서는 실행안됨 
  + `.once`: 이벤트를 한번만 실행 시킴 
  + `.passive`: 기본 이벤트를 취소할 수 없게 함(.preventDefault()를 실행 안되게 함.)
+ `<a id="goodboy" @click.stop="good">좋아요</a>` : 좋아요 버튼을 클릭하면 javascript가 <a>태그가 기본 수행하는 `href="..."` 이벤트를 중지시키고 good() 메서드만 호출한다.
+ `<div v-on:scroll.passive="onScroll">...</div>` : 스크롤의 기본 이벤트를 취소할 수 없게한다.

  
#### 이벤트 버블링과 캡쳐링
```node
<div class="parent" @click="handlerA">
  <div class="child" @click="handlerB"></div>
</div>
```
+ 버블링
  + child를 포함하고 있는 상위의 영역에 영향을 주는 현상
  + child를 클릭했을 때, child->parent 순으로 클릭된다.
  + 버블링을 방지하기 위하여  Vue.js에서는 stop이라는 수식어를 사용하며 이는 `stopPropagation`메소드와 동일한 기능을 한다.
  ```node
  handlerB(e) {
    e.stopPropagation()
  }
  @click.stop="handlerB"
  ```
  + child를 클릭해도 parent는 동작하지 않는다.
+ 캡쳐링
  + 버블링과 반대의 기능을 의도할 때 capture수식어를 이용한다. 상위 요소가 먼저 동작하고 하위 요소로 내려온다.
  + child를 클릭했을 때, parent->child 순으로 클릭된다.
  + 캡쳐링 시 부모 요소에만 동작하려면 `.stop`을 사용하면 된다. => `@click.capture.stop="handlerA"` 


  
  
  
  
 
