### Top K Frequent Elements ###

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

## Example 1: ##
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

## Example 2: ##
```
Input: nums = [1], k = 1
Output: [1]
```

### 풀이 ###

```javascript
var topKFrequent = function(nums, k) {
//     1. map 선언
    let map = new Map;

// 2. nums에 있는 숫자들로 map set해줌 (key: num, value: 빈도값)
    for(let num of nums){
        if(!map.has(num)){
            map.set(num, 0)
        } else {
            map.set(num, map.get(num) + 1)
        }
    }

// 3. 새 arr에 map 값 넣어줌
    let arr = [];
    for([key,val] of map){
        arr.push([key, val]);
    }

// 4. arr의 빈도값 기준으로 내림차순으로 sort해줌
    arr.sort((a,b) => b[1]-a[1]);

// 5. answer 배열 선언
    let answer = [];

// 6. for문으로 k번까지 돌림
// answer 배열에 arr 이중배열 첫번째 값 push해줌
    for(let i = 0; i < k; i++){
        answer.push(arr[i][0]);
    }
    return answer;
};
```
