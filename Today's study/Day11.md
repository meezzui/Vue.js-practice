### 오늘의 일기
---
#### 인스턴스
+ 옵션
<img width="600" src="https://user-images.githubusercontent.com/86812098/156483487-b316e97c-2569-432d-8c3a-92728e690043.png">

#### 컴포넌트
+ 컴포넌트는 HTML 마크업, 자바스크립트 로직을 포함한 하나의 덩어리
+ 캡슐화가 자연스럽게 가능해지고 따라서 재사용이 가능해짐

#### 컴포넌트 등록
+ 전역 컴포넌트
  + 뷰 라이브러리를 로딩하고 나면 접근 가능한 Vue 변수를 이용하여 등록한다. 
  + 전역 컴포넌트를 모든 인스턴스에 등록하려면 Vue 생성자에서 .component()를 호출하여 수행한다.
  + `Vue.component("컴포넌트 이름", { //컴포넌트 내용 })`
  + 컴포넌트 이름: template 속성에서 사용할 HTML 사용자 정의 태그 이름을 의미한다.
  + 컴포넌트 내용: 컴포넌트 태그가 실제 화면의 HTML 요소로 변환될 때 표시될 속성들을 작성한다.
  + 컴포넌트 내용에는 template, data, methods 등 인스턴스 옵션 속성을 정의할 수 있다.
  + 예시
  ```node
  <html>
    <head>
      <title>Vue Component Sample</title>
    </head>
    <body>
      <div id="app">
        <button>컴포넌트 등록</button>
        <coffee></coffee>
      </div>
      <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
      <script>
        Vue.component("coffee", {
          template: "<div>전역 coffee 컴포넌트가 등록되었습니다</div>",
        })
        new Vue({
          el: "#app",
        })
      </script>
    </body>
  </html>
  ```
  + 결과 : 컴포넌트 등록 버튼이 생기고 그 아래 바로 '전역 coffee 컴포넌트가 등록되었습니다'가 보여짐
  + 즉, 저 coffee태그 위치에 들어갈 html내용을 컴포넌트 등록을 통해 한 것이다.
+ 지역 컴포넌트
  + 인스턴스에 components 속성을 추가하고 등록할 컴포넌트 이름과 내용을 정의한다.
  ```node
  new Vue({
      ...
      components:{
          'coffee':coffeeComponent
      }
  })
  ```
  + 전역/지역 예시
  ```node
  <html>
    <head>
      <title>Vue Sample</title>
    </head>
    <body>
      <div id="app">
        <button>app 컴포넌트 등록</button>
        <coffee></coffee>
        <coffee-americano></coffee-americano>
      </div>
      <hr />
      <div id="app2">
        <button>app2 컴포넌트2 등록</button>
        <coffee></coffee>
        <coffee-americano></coffee-americano>
      </div>
      <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
      <script>
        Vue.component("coffee", {
          template: "<div>전역 coffee 컴포넌트가 등록되었습니다 !</div>",
        })

        var coffeeAmericano = {
          template:
            "<div>지역 coffee americano 컴포넌트가 등록되었습니다 !</div>",
        }

        new Vue({
          el: "#app",
          components: {
            "coffee-americano": coffeeAmericano,
          },
        })

        new Vue({
          el: "#app2",
        })
      </script>
    </body>
  </html>
  ```
  + 결과: id가 app으로 되어있는 부분은 전역/지역 모두 다 나오지만 id가 app2라고 되어있는 것은 전역만 나온다.
  + 전역 컴포넌트는 어느 인스턴스에나 사용가능한 컴포넌트이지만 지역 컴포넌트는 특정 인스턴스 내에서만 사용가능한 컴포넌트이다.
  + 그래서 app이라는 유효범위 내에서 설정된 ‘coffee-americano’ 같은 경우는 app이라는 id를 가진 돔 요소에서만 사용가능하고, 전역 컴포넌트로 등록된 ‘coffee-americano’는 어느 뷰 인스턴스에나 사용가능하다.


   
   
   
   
   
   
   
   
   
