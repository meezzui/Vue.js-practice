#### Router 주소로 이동하는 방법
+ `methods`를 사용
+ `methods`에 등록해두고 클릭시 해당 메소드를 호출하여 `$router`를 이동하는 방법
+ `routeTo()`와 같은 메소드를 만들어서 ` $router.push()`와 같은 방법으로 이동 -> 메소드명은 내가 원하는데로 지으면 됨.
```node
this.$router.push('/siteList') // 이동 위치를 입력
this.$router.push({ name: 'SiteList' }) // 해당하는 라우터 이름으로 이동
this.$router.push({ path: '/siteList' }) // 해당하는 pathname을 입력하여 이동
```
+ 파라미터, 쿼리스트링 정보를 함께 전달할 수 있다. -> `this.$router.push({ name: '', query: {}, params: {}})`

#### $router를 이동하는방법
+ $router.push() : 현재 라우트를 변경(기록이 쌓이기 때문에 백버튼을 눌러도 뒤로 안 가짐)
+ $router.replace() : history 객체에 남기지 않고 라우트를 변경(백버튼 누르면 목록으로 감)
+ $router.go() : 앞 또는 뒤 위치로 이동할 수 있음
