#### props 두번 거치기
+ 포함관계
  + `사용단` ⊃ `BaseLayout` ⊃ `Header` 
  + `BaseLayout`과 `Header`가 컴포넌트로 따로 빼져있다.
  + `BaseLayout`에서는 `Header`에서 props로 `BaseLayout`에 정보를 보내고 그것을 `BaseLayout`에서 다시 `props`로 사용단으로 보낸것을 받아 사용한다.
  + 이렇게 하려면 `BaseLayout`에서도 `Header`에서 받은 값을 `props`로 다시 선언해줘서 사용단에 넘겨줘야 한다.  
    주의💥 이렇게 하지 않으면 사용단에서 `Header`에서 선언한 `props`값으로 사용하려고 할 경우 콘솔로 찍어보면 `undefinded`가 뜬다❗❗  
    => 중간에 연결단계가 끈겼기 때문에...😅

+ 실제 해보기😀
  + `Header.vue`

    ```node
    // <ion-toolbar> 태그는 'background-color'가 안 먹힘. '--background'로 해주어야 한다.
    <ion-toolbar :style=" headerChangeColor ? '--background:' + headerChangeColor : ''"></ion-toolbar> 
    props: ['headerChangeColor']
    ```
  + `BaseLayout.vue`
    + `Header.vue`에서 받은 것을 넘겨주기 위해 'props'로 또 선언해준다.  
    ```node
    <Header id="header" :headerChangeColor="changeColor"/>
    props: ['changeColor']
    ```
  + `사용 페이지`
    + `BaseLayout.vue`에서 선언해준 `props값`을 `changeColor="true"` 이렇게 사용하면 되는데 `isTicket`의 값에 따라 색상이 변경되어야 하므로 `changeColor`를 바인딩하여 사용해야 한다.
    ```node
    <BaseLayout :changeColor="isTicket ? '#509DF5' : '#9AA2AB'"></BaseLayout>
    ```
    + 이렇게 바인딩하여 색깔요소를 직접적으로 넣는 것이 가능한 이유🧐
      +   
