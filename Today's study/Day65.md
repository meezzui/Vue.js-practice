#### 흐르는 글자(애니메이션 효과) 적용하기
+ 글자가 오른쪽에서 왼쪽으로 흐르는 애니메이션 효과 넣기
+ 보통 이 효과를 `<marquee>` 태그를 사용해서 표현한다. 하지만 이 태그는 곧 사라지기 때문에 그 대신 css로 이를 나타낸다.
+ 먼저 흐르는 글자를 표현할 텍스트에 클래스명을 준다. => `<div class="top-text-p marquee"><p>{{$t('smartHandsFreeTopBanner')}}</p></div>`
+ 그리고 아래와 같이 css를 적용한다.
```node
.marquee {
  overflow:hidden;
  position:relative;
}

.marquee p:after {
content:"";
white-space:nowrap;
padding-right:50px;
}

.marquee p {
margin:0;
padding-left:600px;
display:inline-block;
white-space:nowrap;
color: white!important;
-webkit-animation-name:marquee;
-webkit-animation-timing-function:linear;
-webkit-animation-duration:10s;
-webkit-animation-iteration-count:infinite;
-moz-animation-name:marquee;
-moz-animation-timing-function:linear;
-moz-animation-duration:10s;
-moz-animation-iteration-count:infinite;
-ms-animation-name:marquee;
-ms-animation-timing-function:linear;
-ms-animation-duration:10s;
-ms-animation-iteration-count:infinite;
-o-animation-name:marquee;
-o-animation-timing-function:linear;
-o-animation-duration:10s;
-o-animation-iteration-count:infinite;
animation-name:marquee;
animation-timing-function:linear;
animation-duration:10s;
animation-iteration-count:infinite;
}
@-webkit-keyframes marquee {
  from   { -webkit-transform: translate(0%);}
  99%,to { -webkit-transform: translate(-100%);}
}
@-moz-keyframes marquee {
  from   { -moz-transform: translate(0%);}
  99%,to { -moz-transform: translate(-100%);}
}
@-ms-keyframes marquee {
  from   { -ms-transform: translate(0%);}
  99%,to { -ms-transform: translate(-100%);}
}
@-o-keyframes marquee {
  from   { -o-transform: translate(0%);}
  99%,to { -o-transform: translate(-100%);}
}
@keyframes marquee {
  from   { transform: translate(0%);}
  99%,to { transform: translate(-100%);}
}
```
+ 위에 있는 css를 그대로 적용하면 흐르는 글자의 애니메이션을 표현할 수 있다.
+ 아래에 영상의 배경색은 `linear-gradient`을 사용하였다. 
  + `background: linear-gradient(to left, #22BDB6,#2EB5C6,#3AADD6,#44A6E4,#4BA0EE,#509DF5);` 
#### 결과

https://user-images.githubusercontent.com/86812098/177341005-02e340d1-2d4d-4d91-98c9-98d7b9446400.mp4
