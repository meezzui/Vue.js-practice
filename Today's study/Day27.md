#### 이미지 슬라이드 + 이미지 갯수만큼 표시되고 현재 이미지가 몇 번인지 보이기
+ swiper 라이브러리를 사용하기 위해서는 import를 해줘야 하는데 전역(main.js)에 import를 해주면 페이지별로 import를 해줄 필요가 없다.
  + `import { Swiper, SwiperSlide } from 'swiper/vue'`, `import 'swiper/css'`,`import '@ionic/vue/css/ionic-swiper.css'`,`import 'swiper/css/pagination'`
+ `<swiper-slide v-for="(place, p) in coursePlaceList" :key="p">` : 일단 for문으로 `coursePlaceList`안에 데이터 만큼 이미지를 돌려준다.
+ `<swiper @slideChange="setCoursePlaceSwiperIdx">`: `slideChange`이 이벤트를 이용하여 슬라이드가 넘어가면서 이미지가 바뀌게 해준다.
  + `setCoursePlaceSwiperIdx` 이 부분은 `setup()` 이 함수 안을 보면 선언되어 있는 것을 알 수 있다.
  ```node
  setup() {
    const coursePlaceSwiperIdx = ref(0) // ref로  첫번째 index가 담긴 것을 coursePlaceSwiperIdx로 선언
    const setCoursePlaceSwiperIdx = (swiper) => { //setCoursePlaceSwiperIdx을 선언해서
      coursePlaceSwiperIdx.value = swiper.realIndex // 슬라이드의 인덱스 값을  coursePlaceSwiperIdx의 value에 넣어준다.
    }
    return { //setup() 함수는 return을 해줘야 선언한 변수를 사용할 수 있다.
      coursePlaceSwiperIdx,
      setCoursePlaceSwiperIdx

    }
  }
  ```
+ `<p>{{coursePlaceSwiperIdx + 1}}/{{coursePlaceList.length}}</p>` : 리스트에 들어있는 이미지 갯수와 현재 이미지가 몇 번째인지를 나타내 준다.
<img width="300px" src="https://user-images.githubusercontent.com/86812098/160957452-8cf0c0aa-9702-41c5-84c5-1e04ea2f396e.png">

+ 이미지 슬라이드 관련 전체 코드
```node
<template>
  <ion-page>
    <BaseLayout id="course_place_detail" :header="{title:'장소 상세', back:true, cart: true}">
      <template #con>
        <!--장소 상세 이미지 슬라이드-->
        <section class="course_place_list_sec">
          <swiper @slideChange="setCoursePlaceSwiperIdx">
            <swiper-slide v-for="(place, p) in coursePlaceList" :key="p">
              <img src="@/assets/image/temp/place-detail.png" alt="tourpass image">
            </swiper-slide>
          </swiper>
          <p>{{coursePlaceSwiperIdx + 1}}/{{coursePlaceList.length}}</p>
        </section>
        <!--장소 상세 이미지 슬라이드 끝 -->
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss" scoped>
#course_place_detail {
  .course_place_list_sec{
    padding: 0; position: relative;
    p {
      display: flex; align-items: center; justify-content: center;
      width: 45px; height: 20px; color: white; text-align: center;
      position: absolute; background: rgba($color: #000000, $alpha: 0.3);
      bottom: 10px; right: 10px; z-index: 999; border-radius: 5px; font-size: 11px;
    }
    img {
      height: 286px; width: 100%;
    }
  }
}
</style>

<script>
import { IonPage } from '@ionic/vue'
import { ref } from 'vue'

export default {
  setup() {
    const coursePlaceSwiperIdx = ref(0)
    const setCoursePlaceSwiperIdx = (swiper) => {
      coursePlaceSwiperIdx.value = swiper.realIndex
    }
    return {
      coursePlaceSwiperIdx,
      setCoursePlaceSwiperIdx

    }
  },
  components: {
    IonPage
  },
  data() {
    return {
      coursePlaceList: [1, 1, 1]
    }
  },
  created() {
  },
  ionViewWillEnter() {

  },
  methods: {

  }
}
</script>
```

