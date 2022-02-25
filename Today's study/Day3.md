### 오늘의 일기
---
#### 비동기 컴포넌트 방식으로 로딩하기(Lazy Loading)
+ 원래 우리가 하던 import 방식으로 import를 하면 파일 개수가 많아질 경우 로딩이 오래 걸린다.(why?🙄 모든 기능을 다 클라이언트로 내려서)
+ 비동기 컴포넌트 방식은 해당 메뉴(등등..)를 클릭했을 때에 클라이언트에 해당 기능을 내려줘서 사용할 수 있게 해준다. 
+ 즉, 클릭하기 전에는 메모리에만 있는 것이다. 사용자가 방문한 순간에 클라이언트로 내려줘서 바로 사용할 수 있게끔 한다.
+ 이렇게 함으로써 애플리케이션의 화면 전환 속도를 빠르게 해준다.  
+ 사용방법 예시
```node
{
  path: '/about',
  name: 'about',
  component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
}
```

✨ 설계의 중요성!! ✨  
+ 그렇다고 모든 파일을 Lazy Load 방식으로 설계를 해야하는 것은 아니다. 
+ Lazy Load도 굉장히 많아지면 초기 화면 로딩할 때 느려질 수 있다.
+ why?🙄 ->  Lazy Load는 파일을 분리해놓고 캐시에 올리기 위해 request를 날리기 때문에 통신이 여러번 일어난다.

#### 설게방법 3가지!!(매우 중요)
+ 원래 우리가 아는 기본적인 import방식
  + 예) 
  ```node
  import MenuBar from '../views/MenuBar.vue'
  
  {
    path: '/menubar',
    name: 'menu',
    component: MenuBar
  }
  ```
  + 모든 기능을 다 클라이언트로 내려서 파일 개수가 많아질 경우 로딩이 오래 걸린다.

+ 방문되는 순간 import되게 하는 방법
  + 처음 로딩 할 때 캐시에 안 올라가고 클릭하는 순간(방문되는 순간) 해당 요소가 캐시에 등록된다.
  + 클릭 전까지는 등록되어 있지 않음.
  + 실무에서 사용될 때 -> 사용자가 접속할지 안 할지 모를 때 / 처음에 로딩할 때 이런 것까지 같이 올라가면 무거워질 것 같을 경우 
  + 사용방법 예시
  ```node
  // 최상위에 vue.config.js파일 만들기
  
  module.exports = {
    chainWebpack: config => {
        config.plugins.delete('prefetch'); // prefetch 삭제
    }
  }
  
  // index.js 파일
  
  {
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
  ```
 
+ prefetch를 사용하는 방법
  + 처음 로딩이 될 때 캐시에 올라오게 됨
  + 실무에서 사용될 때 -> 처음 로딩되자마자 사용자가 바로 그 화면에 방문하게 되는 페이지
  + 미리 캐시에 올려져 있어서 화면 전환 속도가 빠름.
  ```node
  // vue.config.js 파일에 prefeth를 삭제하는 코드를 작성했으니까 webpackPrefeth:true 를 해준다.
  {
    path: '/menubar',
    name: 'menu',
    component: () => import(/* webpackChunkName: "menu", webpackPrefetch:true */ '../views/MenuBar.vue')
  }  
  ```





