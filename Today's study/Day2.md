### 오늘의 일기
---
#### Vue Router
+ Routing 이란?
  + 특정 웹페이지를 접속했을 때 메뉴, 특정 링크를 클릭하면 화면이 전환된다. 
  + 이때 페이지가 이동될 때마다 url주소가 달라진다. 
  + 뷰와 같은 단일 페이지 애플리케이선인 경우에는 클라이언트에서 미리 가지고 있던 페이지를 라우팅을 이용해서 화면을 갱신하게 된다.
  + 즉, 라우팅은 클라이언트에서 url 주소에 따라 페이지가 전환되는 것을 말한다.

+ Vue Router 설치하기
  + cmd창에 `vue add router`라는 명령어를 입력하고 엔터를 누른다.
  + 기존에 src폴더를 보면 assets와 components폴더가 있는데 여기에 router와 views 폴더가 추가되는 것을 확인할 수 있다.
  
+ vue 메뉴 바 추가해보기
  + `src > views` 에 뷰 파일 추가 
  + `index.js`에 추가한 뷰 파일 import 시켜주고 path 경로 설정해주기
  ```java
  import MenuBar from '../views/MenuBar.vue'
  
  {
    path: '/menubar',
    name: 'menu',
    component: MenuBar
  }
  ```
  + `App.vue`에서 router 설정해주기  `<router-link to="/menubar">Menu</router-link>`

🎉 에러발생 🎉  
`Component name "Menu" should always be multi-word  vue/multi-word-component-names` 이런 에러가 발생하였다.  
저 에러를 해석하면 컴포넌트 이름은 단일어가 아닌 멀티단어로 써야 한다는 것이다.  
그래서 컴포넌트 이름을 MenuBar로 바꾸어서 저장했더니 에러없이 실행되었다.  
(컴포넌트 이름을 중간에 바꿀 시에 바꾼 이름이 적용이 잘 안 될수도 있는데 이때는 서버를 다시 껐다가 키면 잘 적용된다.)


