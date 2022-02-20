### 웹팩(webpack)
+ 여러개로 나누어져 있는 파일들을 하나의 자바스크립트 코드로 압축하고 최적화하는 라이브러리이다.

### 바벨(babel)
+ 일종의 컴파일러 같은 개념이다.
+ ES6+ 버전 이상의 자바스크립트나 JSX, 타입스크립트 코드를 하위 버전의 자바스크립트 코드로 변환 시켜 다른 브라우저에서 동작할 수 있도록한다.

### 오늘의 일기
---
#### Vue CLI
+ 터미널에서 명령어를 통해 컴퓨터와 상호작용하는 방식이다.
+ Vue 프로젝트를 생성/관리/빌드하는 기능을 제공한다.
+ 명령어 하나로 Vue 프로젝트에서 필요한 폴더 구조, 파일들, 기본 설정 옵션 등을 자동으로 만들어 준다.
+ 설치 명령어 `npm install -g @vue/cli`

#### Vue CLI를 통해서 뷰 프로젝트 만드는 방법
+ Defalut 옵션으로 생성하기
  + 메뉴 바에 view -> terminal 선택 -> 터미널창 열림 -> 명령어 `vue create 프로젝트명` 입력
  ![image](https://user-images.githubusercontent.com/86812098/154855627-5a503e35-086d-4c98-b1ab-3b92619c3e7a.png)
  + 위와 같은 선택 메뉴가 뜸(첫번째는 Vue 버전3, 두번째는 Vue 버전2, 세번째는 내가 원하는 기능을 선택해서 프로젝트를 생성하는 것을 의미)
  + 가장 최신버전인 Vue3 선택함.
  + 실행 명령어 `npm run serve` -> `http://localhost:8080/`를 브라우저에 쳐서 아래와 같은 화면이 나오면 실행이 잘 된거다.
  <img width=400; src="https://user-images.githubusercontent.com/86812098/154856096-242d55c1-22ee-41bb-8f44-c9f9b96fc533.png">
  
  + 설치된 프로젝트 구조
    + `node_modules` : npm으로 설치된 패키지 파일들이 모여있는 디렉토리
    + `public` : 웹팩을 통해 관리되지 않는 정적 리소스가 모여있는 디렉토리
    + `src > assets` : 이미지, css, 폰트 등을 관리하는 디렉토리
    + `src > components` : Vue 컴포넌트 파일이 모여있는 디렉토리
    + `App.vue` : 최상위(Root) 컴포넌트( 🙄 이 안으로 우리가 개발하는 컴포넌트들이 들어간다고 생각하면 됨)
    + `main.js` : 가장 먼저 실행되는 자바스크립트 파일로써 vue 인스턴스를 생성하는 역할을 함( 🙄 `npm run serve`라는 명령어를 사용하여 서버를 실행하는데 그때 main.js가 가장먼저 실행이 되고 main.js에서 내가 개발한 뷰 컴포넌트들이 실행이 된다고 생각하면 됨)
    + `.gitignore` : 깃허브에 업로드할 때 제외할 파일 설정
    + `babel.config.js` : 바벨 설정 파일
    + `package-lock.json` : 설치된 패키지에 디펜던시 정보를 관리하는 파일
    + `package.json` : 프로젝트에 필요한 패키지를 정의하고 관리하는 파일( 🙄 `dependencies`: 운영단에서 필요한 디펜던시 모음, `devDependencies`: 개발에 필요한 패키지들이 모여있음)
  + 파일별 코드 설명
    + `main.js` 
      ```node
      import { createApp } from 'vue'
      import App from './App.vue' // 기본으로 생성되는 App.vue 파일 불러오기
      createApp(App).mount('#app') // createApp(App): 뷰에서 import시킨 createApp을 App.vue를 import한 것을 넣어줌.
                                  // mount('#app') : index.html을 통해서 뷰 컴포넌트들이 사용자에게 제공됨. index.html의 body에 id를 가리킴.
                                  //개발한 뷰 컴포넌트들이 이 안에서 실행됨.
      ```
    + `App.vue`(보여지는 실제 화면)
      ```node
      <template>
        <img alt="Vue logo" src="./assets/logo.png"> // 로고 이미지
        <HelloWorld msg="Welcome to Your Vue.js App"/> // HelloWorld 라는 뷰 파일을 import 시킴 -> 그러면 컴포넌트 파일 안에 있는 HelloWorld.vue의 내용들이 화면에 보여짐.
      </template>

      <script>
      import HelloWorld from './components/HelloWorld.vue'

      export default {
        name: 'App',
        components: {
          HelloWorld
        }
      }
      </script>
      ```
+ Manually select features 옵션으로 셍성하기(내가 원하는 기능을 선택해서 프로젝트를 생성)
+ Vue 프로젝트 매니저로 생성하기
