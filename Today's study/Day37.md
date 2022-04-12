#### 배열 관련 함수
+ `some()` 🧡주어진 조건을 만족하는지 여부를 검증🧡
  + 배열 안의 어떤 요소라도 주어진 판별 함수를 통과하는지 테스트한다.(예- 체크가 되었는지)
  + 배열 안의 조건이 하나라도 ture이면 true를 반환(그니까 하나라도 체크되있으면 동작하는 거지)
  + 구조 -> `arr.some(함수명 => 함수명.조건)`
  + 예시(button 활성화)
  ```node
  computed: {
    buttonValidation() {
      return this.couponChoiceList.some(cpn => cpn.checked)
    }
  }
  ```
+ `every()` 🧡주어진 조건을 만족하는지 여부를 검증🧡
  + 조건이 모두 true일 때만 true를 반환한다.
  + 하나라도 false이면 반환 값은 false이다.
  + 구조 -> `arr.every((함수명)=> 함수명.조건)`
  + 예시
  ```node
  var objArr = [{name: '철수', age: 10}, {name: '영희', age: 10}, {name: '바둑이', age: 2}]
  
  console.log(objArr.every((item)=> item.age>5)); //false (바둑이 탈락!)
  console.log(objArr.every((item)=> item.age>1)); //true
  ```
+ `map()`
  + 모든 배열의 값에 Function을 실행한다. 그리고 Function에서 나온 값을 저장해서 새로운 배열로 만든다.
  + 구조 -> arr.map(함수명 => 함수명에 기능)
  + 예시
  ```node
  var numbers = [ 1,2,3,4,5,6,7,8,9];
  var newNumbers = numbers.map(number =>number *2);
  ```
  + Array안에 Array가 있는 경우🎈
  ```node
  var numbers = [[1,2,3],[4,5,6],[7,8,9]]; //array안에 array가 있는 경우
  var newNumbers = numbers.map(array => array.map(number => number *2));
  ```
+ `filter()` 💛배열의 특정 값 찾기💛
  + 특정 조건을 만족하는 새로운 배열을 필요로 할 때 사용
  + 즉, 내가 원하는 조건의 요소들만 뽑아내고 싶을 때 사용 -> 원하는 조건을 만족하는 새로운 배열을 반환
  + 다른 함수와의 조합성도 좋아 map, reduce와 같은 다른 함수와 함께 자주 쓰인다.
  + 구조 -> arr.filter(함수명 => 함수명에 기능)
  + 예시
  ```node
  const guys = [ { name: 'YD', money: 500000 }, { name: 'Bill', money: 400000 }, { name: 'Andy', money: 300000 }, { name: 'Roky', money: 200000 } ]; 
  const rich = guys.filter(man => man.money > 300000); //300000 이상을 가진 사람들은?
  const richNames = guys.filter(man => man.money > 300000) .map(man => man.name) // 300000 이상을 가진 사람들의 이름은?
  ```
+ `find()` 💛배열의 특정 값 찾기💛
  + 주어진 판별 함수를 만족하는 첫 번째 요소의 값을 반환한다.( 그런 요소가 없다면 undefined를 반환)
  + 다른 애들과 다르게 number를 반환한다.
  + .filter()와 다르게 찾고 싶은 요소를 검색한 후에는 즉시 검색을 종료
  + 장점: 큰 데이터를 처리할 때 검색 속도가 빠르다
  + 단점: 여러개의 데이터를 검색할 때는 활용하기가 까다롭다.
  + 구조 -> arr.find(함수명 => 함수명에 기능)
  + 예시
  ```node
  const array1 = [5, 12, 8, 130, 44];
  const found = array1.find(element => element > 10);
  ```

