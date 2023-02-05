# Hash Table

### 해시 테이블이란?
- key-value 쌍을 가진 자료구조 (예시: colors["pink"])
- 배열보다 search, insert, remove에서 속도가 빠르다.
- 프로그래밍 언어마다 고유의 해시 테이블을 가지고 있다.
    - Python : Dictionaries
    - JS : Objects, Maps _(Objects는 각종 제약이 있지만 기본적으로 Hash table로 분류한다.)_
    - Java, Go, Scala : Maps
    - Ruby : Hashes




### 해시함수(Hash function)
- value를 찾기 위해 key를 배열의 index로 변환하는 작업
- 개인정보 보호 및 저장, 사이트 로그인, 암호화폐 작업 등에 사용
- 특징
    - 입력값 길이에 상관없이 결과값은 비슷한 길이로 출력된다.
    - 단방향 함수로, 결과값을 가지고 입력값을 찾아낼 수 없다.
- 좋은 Hash Function의 조건
    - 속도가 빨라야 한다.
    - 키-값을 중복없이 고르게 분배해야 한다.
    - same input, same output

### Basic 해시 함수 
```javascript
function hash(key, arrayLen) {
  let total = 0;
  for (let char of key) {
    // map "a" to 1, "b" to 2, "c" to 3, etc.
    let value = char.charCodeAt(0) - 96
    total = (total + value) % arrayLen; // 나머지 연산 이용
  }
  return total;
}
```
- 해당 함수의 문제점
 1) String만 처리 (boolean, 숫자 등 다른 데이터 처리 못함)
 2) 연산 속도가 일관적이지 않음 - String 길어지면 속도도 느려짐 
 3) 데이터가 겹치기 쉬움 (랜덤성 필요)

### 해시 함수 개선
```javascript
function hash(key, arrayLen) {
  let total = 0;
  // 소수 추가해서 데이터 충돌을 줄임
  let WEIRD_PRIME = 31;
  // 최소값을 이용해서 key String 길이를 100까지만 돌게 함
  for (let i = 0; i < Math.min(key.length, 100); i++) {
    let char = key[i];
    let value = char.charCodeAt(0) - 96
    total = (total * WEIRD_PRIME + value) % arrayLen;
  }
  return total;
}
```

### 충돌 해결하기
1. Separate Chaining (개별 체이닝)
- 배열, linked list 등을 이용해서 개별 저장하는 방법
- 배열을 사용하는 경우 사용은 쉽지만 중첩 배열 구조가 나온다.
2. Linear Probing
- 각 위치에 하나의 데이터만 저장하는 규칙을 그대로 지킨다.
- 충돌이 발생하면 다음 빈 칸이 어디인지 확인해서 저장한다.

### Set
- key와 value를 받고 key를 해시 처리한 뒤, 개별 체이닝을 이용해 key-value 쌍을 넣는다.
```javascript
  set(key,value){
    let index = this._hash(key);
    if(!this.keyMap[index]){
      this.keyMap[index] = [];
    }
    this.keyMap[index].push([key, value]);
  }
```
### Get
- key를 받고 해시 처리한 뒤, 해쉬 테이블에서 key- value쌍 출력
- key가 없는 값이면 undefined 출력
```javascript
  get(key){
    let index = this._hash(key);
    if(this.keyMap[index]){
      for(let i = 0; i < this.keyMap[index].length; i++){
        if(this.keyMap[index][i][0] === key) {
          return this.keyMap[index][i][1]
        }
      }
    }
    return undefined;
  }
```

### keys
배열에 있는 모든 key 출력하는 메소드
```javascript
  keys(){
    let keysArr = [];
    for(let i = 0; i < this.keyMap.length; i++){
      if(this.keyMap[i]){
        for(let j = 0; j < this.keyMap[i].length; j++){
          if(!keysArr.includes(this.keyMap[i][j][0])){
            keysArr.push(this.keyMap[i][j][0])
          }
        }
      }
    }
    return keysArr;
  }
```

### values
배열에 있는 values 출력하는 메소드
```javascript
  values(){
    let valuesArr = [];
    for(let i = 0; i < this.keyMap.length; i++){
      if(this.keyMap[i]){
        for(let j = 0; j < this.keyMap[i].length; j++){
          if(!valuesArr.includes(this.keyMap[i][j][1])){
            valuesArr.push(this.keyMap[i][j][1])
          }
        }
      }
    }
    return valuesArr;
  }
```

### 빅오 복잡도
- 평균적인 경우 상수값을 가진다.
  - Insert: O(1)
  - Deletion: O(1)
  - Access: O(1)
