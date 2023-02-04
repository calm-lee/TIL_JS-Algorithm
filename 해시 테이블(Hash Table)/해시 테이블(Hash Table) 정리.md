
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
