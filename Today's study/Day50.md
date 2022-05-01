 #### vue-swiper <style lang="scss" scoped> 적용안되는 이슈
 + `scss scoped`를 통해서 `swiper`를 커스터마이징 하려고 하는데 `swiper-pagination`의 스타일 변경이 적용되지 않는 문제가 발생했다.
 + 여기서 잠깐❗❗ `scoped`이란?🧐
   + `<style>`태그에 `scoped`속성이 있는 경우 해당 `CSS`는 현재 구성 요소의 요소에만 적용된다. 다시말해, 해당 vue 페이지에서만 적용된다는 것이다.
 + `swiper`의 `bullet`을 커스터 마이징 하기 위해 개발자 모드를 켜서 `bullet`에 해당하는 `css`요소 이름을 찾아서 커스터마이징을 해주었다.
 + 그런데 `swiper-pagination`을 커스터마이징하려고 했는데 css가 전혀 먹히지 않는 문제가 발생하였다.
 ```node
  <style lang="scss" scoped>
  .swiper {
    --bullet-background: black;
    --bullet-background-active: #fff;
    --swiper-pagination-bullet-width: 6px;
    --swiper-pagination-bullet-height: 6px;
    .swiper-pagination{ //안 먹힘...ㅠㅠ
      position: unset;
      margin-top: 20px;
    }
  }
  </style>
  ```
 + 이유는????😲😲😲
   + `scoped css`를 사용하면 `data-v-f3f3eg9`와 같은 값이 태그에 추가되는데, 이를 통해 현재 컴포넌트에만 스타일을 적용한다.
   + `data-v-~`가 추가된 태그는 `scoped css`가 잘 적용되었지만, `swiper/vue`에 의해서 동적으로 추가된 엘리먼트인 `swiper-pagination`은 `scoped css`가 적용되지 않았다.

   <img width="400" src="https://user-images.githubusercontent.com/86812098/166105525-1273b531-4c50-4aa6-ae45-1425656af461.png"/>

 + 그래서 아래 코드 처럼 따로 글로벌 css를 적용해서 스타일을 변경해줬다.
 ```node
 <style lang="scss">
 .ticket-back-box{
    .swiper-pagination {
      position: unset !important;
      margin-top: 20px;
    }
  }
</style>
```
+ 여기서 주의사항❗❗
  + 아무래도 `<style lang="scss">` 이렇게 사용하면 전역에 이 css가 적용되기 때문에 다른 곳에도 스타일이 변경될 위험이 있다. 
  + 따라서 해당 컴포넌트 요소의 클래스 명으로 한번 묶어주는 것이 더 안전하다.
 
+ 막간의 코드 설명😉
  ```node
  <swiper
    class="swiper"
    slidesPerView="1"
    :modules="modules"
    :pagination="{  }">
   ```
   + `slidesPerView="1"`: 슬라이드 한 칸당 보여줄 이미지 수
   + `:modules="modules"`: modules를 바인딩하고 아래 data()부분에 변수를 선언하여 `modules: [Pagination]`이렇게 배열안에 pagination을 넣어준다.
   + `:pagination="{  }"`이 바인딩을 통해 빈 오브젝트 안에 슬라이드 갯수만큼 들어와 그 수 만큼 bullet 갯수가 보이게 된다.
 + 최종코드
 ```node
 <template>
  <ion-page>
    <BaseLayout :header="{title:title, back:true, backcolor:'blue'}">
      <template #con>
        =================================(생략)==================================
          <div>
            <swiper
              class="swiper"
              slidesPerView="1"
              :modules="modules"
              :pagination="{  }">
              <swiper-slide v-for="(ticket, t) in ticketList" :key="t">
                <div class="ticket-backround">
                  <div class="ti-detail-box">
                    <div>
                      <span class="ti-detail-title">신참떡볶이</span>
                    </div>
                    <div class="ti-detail-img">
                      <img src="@/assets/image/temp/ticketDetailIMenuImage.png" alt="ticket_pro_img"/>
                    </div>
                    <div>
                      <p class="ti-detail-contents">떡볶이+순대+어묵 세트</p>
                    </div>
                    <div class="ti-detail-count-box">
                      <div>
                        <p class="ti-detail-count">수량</p>
                      </div>
                      <div>
                        <span class="ti-detail-count-number">1</span>
                      </div>
                    </div>
                    <div class="stemp-box" v-if="isTicket===false">
                      <img src="@/assets/image/temp/using-complete.png" alt="" class="using-complete-img"/>
                    </div>
                    <div class="ticket-bottom-box">
                      <div class="ticket-bottom-img1">
                        <img src="@/assets/image/temp/ticketComment.png" alt="" class="comment-img"/>
                      </div>
                      <div class="ticket-bottom-img2">
                        <img src="@/assets/image/temp/stemp.png" alt="" class="stemp-img"/>
                      </div>
                    </div>
                  </div>
                </div>
              </swiper-slide>
            </swiper>
          </div>
          ===================================(생략)==================================
      </template>
    </BaseLayout>
  </ion-page>
</template>

<style lang="scss">
.ticket-back-box{
  .swiper-pagination {
    position: unset !important;
    margin-top: 20px;
  }
}
</style>

<style lang="scss" scoped>
=====================(생략)========================
.swiper {
  --bullet-background: black;
  --bullet-background-active: #fff;
  --swiper-pagination-bullet-width: 6px;
  --swiper-pagination-bullet-height: 6px;
}
=====================(생략)========================
</style>

<script>
import { IonPage } from '@ionic/vue'
import { defineComponent } from 'vue'
import { Pagination } from 'swiper'

export default defineComponent({
  name: 'VoucherDetailPage',
  components: {
    IonPage
  },
  data() {
    return {
      title: '교환권/티켓',
      isTicket: false,
      ticketList: [1, 1, 1, 1],
      tourpass: true,
      modules: [Pagination]
    }
  },
  created() {
  }
})
</script>
 ```
#### swiper 사용법
+ `npm i swiper` 로 swiper를 설치해준다.
+ swiper를 사용할 페이지에 import를 해준다. 예) `import { Pagination } from 'swiper'` : pagination을 사용하겠단 의미!!!
+ `<swiper></swiper>`, `<swiper-slide></swiper-slide>` 안에 요소들을 넣어주면 된다.
