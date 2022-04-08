#### 배열에 사용되는 함수
+ `push()`: 배열에 데이터 추가 (맨 끝 인덱스부터 추가됨)
  + `this.taxList.push({ ...this.blankTaxForm })` : 배열 끝에 object를 추가해줌. `addTaxObject()`함수가 작동되면 배열 끝에 object가 추가 됨
  + `{ ...this.blankTaxForm }`
    + 그냥 `this.taxList.push({ this.blankTaxForm })` 이렇게 해주면 object를 추가할 때 배열에 계속 똑같은 값이 들어가게 된다.
    + `blankTaxForm: { date: '', selected: 'placeholder' }` 이 형식의 object를 복사해 와야 요소가 배열에 추가되면 각각의 요소에 다른 해당 데이터가 들어가게 된다.
    + 여기서 `{...}`을 사용하면 그 object 형태를 그대로 복사해 올 수 있다.
+ `slice()` : 배열의 특정 인덱스에 있는 값을 반환 (배열의 내용이 변환되지 않음)
  + 즉, 배열을 잘르는 역할
  + `arr.slice(1, 3); ` : 배열의 arr[1] ~ arr[3] 까지(arr[3]은 미포함)를 복사한, 새 배열을 리턴 // b,c
  + ` arr.slice(1); ` : 두번째 파라미터인 end 값이 입력되지 않으면, 시작 index부터 배열의 끝까지를 복사한, 새 배열을 리턴 // b,c,d
  + `arr.slice(-3, -1); ` : begin index나 end index가 음수이면, 배열의 끝에서부터의 길이를 나타냄 // b,c
+ `splice()` : 배열의 특정 인덱스에 있는 값을 변경 또는 삭제 (배열의 내용이 변경됨)
  + `this.taxList.splice(index, 1)` : 해당 인덱스의 1개 요소 삭제. 즉, `taxList`라는 배열에 들어있는 선택한 object를 삭제함
+ `pop()`: 배열의 마지막 인덱스의 값을 꺼냄 (배열의 내용이 변경됨) => `arr.pop()`
+ `shift()`: 배열의 첫번째 인덱스의 값을 꺼냄 (배열의 내용이 변경됨) => `arr.shift()`
```node
export default {
  name: '',
  components: {
    IonPage, IonButton, IonModal, IonDatetime
  },
  setup() {
  },
  data() {
    return {
      blankTaxForm: { date: '', selected: 'placeholder' },
      taxList: []
    }
  },
  created() {
    this.taxList.push({ ...this.blankTaxForm })
  },
  methods: {
    addTaxObject() {
      this.taxList.push({ ...this.blankTaxForm })
    },
    deleteData(index) {
      this.taxList.splice(index, 1)
    }
  }
}
```
