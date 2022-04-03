### 가상 DOM이란?
+ DOM(Document Object Model)이란?
  + 다큐먼트(웹페이지)를 객체로 표현하는 모델로서 javascript가 이용할 수 있는 (메모리에 보관할 수 있는) 객체이다.
  + 즉. DOM은 HTML과 스크립트언어(Javascript)를 서로 연결해주는 역할을 한다.
+ 브라우저가 DOM을 랜더링하는 순서
  + HTML 파싱하여 `DOM tree` 생성
  + css 파일, inline 스타일 파싱-> DOM+CSS=`render tree` 생성
  + 스크린에서 좌표에 따라 위치 결정(Layout)
  + 실제 화면에 그리기 (Paint)
+ DOM 변경의 단점
  + 돔 트리가 수정될 때마다 렌더 트리가 계속해서 실시간으로 갱신된다.
  + 즉, 화면에서 10개의 수정사항이 발생하면 수정할 때마다 새로운 랜더 트리가 10번 수정되면서 새롭게 만들어지게 되는 것
  + 불필요한 렌더링으로 인해 속도 저하를 일으킴!!😔
+ 가상 DOM이란?
  + DOM의 단점을 해결하기 위해 탄생!!😉
  + 실제 DOM의 추상화 버전으로 데이터가 업데이트 되면 전체 UI를 가상돔에 리렌더링한다.
  + 변화 전의 가상돔과 변화된 가상돔을 비교한다.
  + 바뀐 부분만 실제 돔에 적용을 함으로서 렌더링이 한 번만 일어나게 된다.
  + 즉, 웹 브라우저 DOM을 직접 수정하지 않고 가상 DOM을 활용하여 필요 부분만 수정을 진행!!!🎃
### ref란?
+ 가상 DOM을 조작하는 함수이다.
+ 기존 `getElementsById`, `getElementsByClassName`, `Document.creatElement()`을 통해 직접 DOM에 접근해서 조작하는 함수를 대신해 사용됨
+ vue에선 위의 메서드들 사용 자제!!
+ why?🧐
  + 웹 브라우저 단에서는 DOM에 변화가 일어나면 웹 브라우저가 CSS를 다시 연산/레이아웃/구성하며 페이지 리페인트를 하기 때문에 시간 지연이 발생하게 됨
+ 사용
  + JavaScript로 Props&Event 거치지않고 자식컴포넌트 직접 접근
    + 부모에서 자식 ref에 바로 접근 가능함
+ 주의!!🧨
  + ref 속성은 컴포넌트가 랜더링이 된 이후 적용되기 때문에 반응형으로 구성하기 위한 computed 나 template 에서 사용하면 안됨!!
+ 사용 방법
  + 사용하려는 태그에 ref 속성을 달아주고 script를 통해서 접근하면 됨
  ```node
  <input ref="input">
  
  methods: {
    // Used to focus the input from the parent
    focus: function () {
      const target = this.$refs.input // 자식의 ref 접근할때도 this.$refs.자식ref 로 하면 됨!!
      target.focus()
    }
  }
  ```

### Vue instance code
```node
new Vue({
  el : //Vue로 만든 화면이 표현되는 tag의 id, vue instance 적용 지점, 외부 접근 시 "변수.$el"
  data : //instance의 데이터 속성, 여러 데이터 저장 및 활용
  methods : //instance의 이벤트 정의
  template : //화면에 표시될 HTML/CSS 등 요소 정의
  created : //instance 라이프 사이클을 커스터마이징
});
```
+ `template`: Vue 화면에 표시될 html element나 style들을 설정, 미리 만들어둔 html(템플릿)
+ `methods`: 메소드들을 객체로 등록, 이벤트 처리 등 동적 작업 처리 시 사용, 함수처럼 호출하여 실행
+ `computed`: Vue instance에 존재하는 데이터 조작시에 주로 사용, methods와 마찬가지로 함수처럼 호출하여 실행. data 객체 내 속성에 변화가 발생할 때마다 반응
+ `watch`: data 객체 내의 데이터를 모니터링 할 수 있으며, 특정 데이터에 변화가 발생하면 후속 처리 가능
+ `props`: 다른 컴포넌트와 데이터를 직접적으로 전달하는데 사용, `부모 컴포넌트 → 자식 컴포넌트`로 데이터를 전달할 때 사용

### vue 인스턴스 생명주기(Life Cycle)
+ 인스턴스 생명주기란?
  + 인스턴스의 상태에 따라 호출할 수 있는 속성들을 의미한다.
+ 인스턴스 생명주기를 사용해야 하는 이유?
  + 개발을하다보면 한 프로젝트에 components 폴더에 많은 component 파일들이 생긴다.
  + 이를 효율적으로 다뤄주지 않으면 화면이 느려지는 등 좋은 사이트를 만들 수 없다.
  + 즉, component들을 효율적으로 사용하기 위해 사용하는 것이 `인스턴스 생명주기`이다.

#### 생명주기 종류
+ 생명주기 종류에는 크게 4가지가 있다.  🎉 `created`, `mounted`, `updated`, `destroyed` 🎉
+ 생성단계
  + beforeCreate()
    + 라이프사이클 중에서 가장 맨 처음 실행된다.
    + 아직 컴포넌트가 DOM에 추가되기 전이기 때문에 DOM에 접근을 하게 되면 에러가 발생한다.
    + 즉, 가장 먼저 실행되는 data()와 events가 세팅되지 않은 상태이다.
  + 🧡created🧡
    + `created()`는 `data()` 변수와 `events` 메서드가 활성화되어 접근할 수 있다.
    + 즉, `@click` 이벤트 같은 메서드를 사용할 수 있다는 것이다.
  + 실행순서
    + `beforeCreate() => data() => created()` 순으로 실행된다.
    + 아직 템플릿과 가상 DOM에는 접근할 수 없다.
+ 장착단계
  + beforeMount()
    + template 태그가 실행된 후 실행된다.(즉, html 태그를 등록)
    + 템플릿과 렌더 함수들이 컴파일된 후에 첫 렌더링 일어나기 직전에 실행된다.
    + 만약에 초기 렌더링 직전에 DOM을 변경하고자 한다면 이 생명주기를 활용하면 된다. 하지만 컴포넌트 초기에 설정해야 할 데이터들은 `created`단계에서 해야한다.
  + 🧡mounted🧡
    + 템플릿과 렌더링 된 돔에 접근할 수 있는 단계!!🤗
    + 주의!!🧨 부모와 자식 관계에서 컴포넌트를 렌더링 할 때 자식 컴포넌트가 부모 컴포넌트보다 먼저 Mounted가 실행됨!!
  + 실행순서
    + `beforMount() => <template>{{onTemplate() => 템플릿 실행}} </template> => mounted()` 순으로 실행
    ```node
    <template lang="ko">
      <div>
        {{onTemplate()}}
        버튼 이름 :
        <button @click="onClick">
           {{name}}
        </button>
      </div>
    </template>
    
    export default {
      name: "CommonButton",
      props: {
        name: String,
      },
      methods: {
        onClick() {
          console.log(this.name, "버튼 클릭");
        },
        onTemplate() {
          console.log("템플릿 실행");
        },
      },
      beforeMount() {
        console.log("beforeMount");
      },
      mounted() {
        console.log("ButtonComponent Mounted");
      },
    };
    ```
    + 실행 결과: 콘솔창에 `탬플릿 실행 => ButtonComponent Mounted` 순서로 나옴
+ 수정단계
  + beforeUpdate()
    + 컴포넌트의 데이터가 변하여 업데이트 시작될 때 실행 된다.
    + 즉, DOM이 재 렌더링 되고 패치되기 직전에 실행된다.
    + 여기서 값을 변경해도 변경되지 않는다.
  + 🧡updated()🧡
    + 컴포넌트가 데이터가 변하여 재 렌더링이 일어난 후에 실행된다.
    + DOM이 업데이트 완료된 상태이며 연산과 기능을 할 수 있지만 여기서 상태를 변경하면 무한루프에 빠질 수 있으니 조심해야 한다.
+ 소멸단계
  + beforeDestroy()
    + 소멸(뷰 컴포넌트 제거)되기 직전에 호출된다.
    + 컴포넌트가기본 모습과 기능을 그대로 가지고 있고 이벤트와 같은 부분을 제거할 때 사용
  + 🧡destroyed()🧡
    + 소멸된 후에 호출된다.
    + Vue의 모든 `v-bind`가 해제되고 모든 이벤트 리스너가 제거되며 모든 하위 Vue 컴포넌트도 삭제된다.

### computed vs methods
+ computed : 데이터 연산들을 정의하는 영역
  + 데이터 값의 변화에 따라 자동으로 연산 수행
  + 데이터가 변경되지 않는 한 이전의 계산값 보유(캐싱⭕), 필요시 반환 → 동일한 연산을 반복수행 하지 않음
  + 복잡한 연산을 반복 수행해서 화면에 나타내야한다면 computed 속성 이용!!
+ method : 메소드 호출시에만 해당 로직 수행, 수행할 때마다 연산(캐싱❌)

### Computed 캐싱
+ 캐싱이란? 
  + 한 번만 로직이 실행되고 똑같은 값을 반환하는 것
+ computed를 통해 만든 계산된 데이터는 `캐싱`이라는 기능을 통해서 한 번 연산해놓은 값을 반복적으로 출력할 때 다시 한번 연산하지 않는다.
+ why?🧐 저장되어 있는 캐싱된 값으로 그대로 해당하는 내용을 화면에 출력하기 때문이다.
+ 이 기능은 데이터를 최적화하는 용도로 자주 사용된다.






