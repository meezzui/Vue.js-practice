#### 이미지 비율로 지정
+ 이미지 크기를 800*640 픽셀이 아닌 비율로 지정하기
+ `800:640`은 계산해보면 5:4이다.(800이 가로, 640이 세로)
+ 이미지를 비율로 지정한다는 말이 처음에 어떻게 해줘야 하는건지 방향을 못 잡았다.
+ `이미지를 비율로 지정`한다는 말은 이미지의 원래 크기와는 관계없이 일정한 비율로 이미지가 보이게 한다는 의미이다.
+ 처음엔 `vw`나 `vh`를 생각했었는데 이 방법으로는 원하는 결과가 나오지 않았다.😤
+ 이미지를 비율로 지정하려면 어떻게 해야할까??🧐
  + `aspect-ratio` 속성을 사용한다❗❗
  + `aspect-ratio`이란 이미지나 동영상을 비율대로 줄이거나 늘리는 데 사용된다.
  + 예전에는 css에서 비율로 조절할 수 있는 css 프로퍼티가 존재하지 않았다.
    + 예전에는 이미지 비율을 `padding`이나 `calc()`를 사용하여 조정했었다.
  + 하지만 css에서 최신 기능으로 `aspect-ratio` 라는 속성을 지원하게 되었다.(아마 2021년에 도입..😅)👍👍👍

+ 사용법📔
  + `aspect-ratio: width / height;` : 왼쪽엔 가로비, 오른쪽엔 세로비를 적는다.
  + 예) `aspect-ratio: 5/4;`
+ 주의❗❗
  + css를 사용할 때 css의 프로퍼티 이름을 쓰면 살짝 색깔이 변하는데 `aspect-ratio`는 색깔이 딱히 변하지 않는다. 그렇다고 해서 적용이 안 되는것이 아니기에 걱정할 필요가 없다😉
  + 그리고 `swiper`안에 `img태그`에 적용할 때는 각 페이지에 적용하려고 하면 안 될 수 있다. 그럴 때는 전역에 있는 css에 각 페이지의 `template`의 가장 밖을 감싸고 있는 div의 class나 id명으로 감싸줘서 따로 적용해주어야 한다.
    + `template의 가장 밖을 감싸고 있는 div의 class`로 먼저 감싸준 이유는??😎 
      + 전역 css페이지이기 때문에 이렇게 해주지 않으면 모든 swiper에 다 적용되어 버린다구😥
  + 예시
  ```node
  #pro_detail_contain, .pc-left-sec, #course_detail, #course_place_detail, .entire-wrapper{
    .swiper-button-prev, .swiper-button-next{
      display: none;
    }
    .swiper{
      .swiper-wrapper{
        .swiper-slide{
          img{
            aspect-ratio: 5/4;
            width: 100%;
          }
        }
      }
    }

  }
  ```


