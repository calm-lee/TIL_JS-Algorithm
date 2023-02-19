# Graph Traversal
- 그래프를 순회할 때는 시작점을 찍어줘야 함

## DFS (Dept First Search)
- 형제 노드보다 자식노드를 먼저 방문 
- 깊이 우선 - 거리에서 멀어진다는 뜻
- 종류
	i. 전위
	ii. 중위
	iii. 후위


## 재귀적 용법의 DFS

- 방문하지 않은 정점이라면 재귀형으로 호출함

#### 의사코드
1. 시작점 노드를 입력하는 함수 작성
2. 빈 배열을 만들어서 최종 결과 저장
3. 방문한 정점을 저장할 수 있는 객체 생성
4. 정점을 입력하는 헬퍼 함수 작성
	-  정점이 비었으면 return 해줘야 함
	- 입력한 정점을 방문 객체와 결과값 배열에 넣어줘야 함
	- 해당 정점의 이웃 리스트에 대해 루프를 돌림
	- 방문하지 않았다면, 해당 정점에 대해 재귀적으로 이 헬퍼함수를 호출하도록 함
5. 시작 정점에 대해 헬퍼 함수를 호출하도록 함


### 실제 코드
```javascript
    depthFirstRecursive(start){
        const result = [];
        const visited = {};
        const adjacencyList = this.adjacencyList;

        (function dfs(vertex){
            if(!vertex) return null;
            visited[vertex] = true;
            result.push(vertex);
            adjacencyList[vertex].forEach(neighbor => {
                if(!visited[neighbor]){
                    return dfs(neighbor)
                }
            });
        })(start);

        return result;
    }

})(start);

  

return result;

}

```


## 반복적 용법의 DFS (Iterative)
- 호술 스택 대신 그냥 스택을 사용 

### 의사코드
1. 시작점 입력하는 함수 작성
2. list/array를 이용해서 정점을 돌 수 있는 stack 생성
3. 마지막에 반환할 result 배열 작성
4. 방문한 정점을 넣을 객체 생성 
5. 시작 지점의 정점을 스택에 넣어주고 방문 표시를 해줌
6. Stack이 비어있지 않으면
	 - 다음 정점을 stack에서 pop해줌
	 - 정점이 방문되지 않은 상태라면
		 - 방문된 걸로 표시
		 - result list에 더해줌
		 - 그 정점의 이웃들을 stack에 넣어줌
7. result array를 반환

### 실제 코드

```javascript
    depthFirstIterative(start){
        const stack = [start];
        const result = [];
        const visited = {};
        let currentVertex;

        visited[start] = true;
        while(stack.length){
            currentVertex = stack.pop();
            result.push(currentVertex);

            this.adjacencyList[currentVertex].forEach(neighbor => {
               if(!visited[neighbor]){
                   visited[neighbor] = true;
                   stack.push(neighbor)
               } 
            });
        }
        return result;
    }

```


## 너비 우선 그래프 순회 (BFS)
- 노드의 인접점을 수평으로 내려가는 방법
- 깊이에 따라 높이가 1, 2...

### 의사코드
- Stack 대신 Queue 이용
- First In, First Out (shift 이용) <-> 깊이 우선: 가장 먼저 들어간 것이 가장 나중에 나옴
1. 시작점을 받는 함수를 생성
2. 시작점을 받는 queue 생성 (array 이용해도 됨)
3. 방문한 node를 받는 array 생성
4. 방문한 node를 저장하는 객체 생성
5. 시작점을 방문한 것으로 표시
6. queue가 비어 있지 않으면 계속 loop
7. Queue에서 첫 번째 정점을 제거하고 방문 노드 array에 push
8. Adjacency list를 순회
9. 방문 노드 객체에 아무 것도 없으면, 해당 정점을 방문한 것으로 표시하고 enqueue함
10. looping을 끝냈으면 array 반환

### 실제 코드

```javascript
    breadthFirst(start){
        const queue = [start];
        const result = [];
        const visited = {};
        let currentVertex;
        visited[start] = true;

        while(queue.length){
            currentVertex = queue.shift();
            result.push(currentVertex);
           

            this.adjacencyList[currentVertex].forEach(neighbor => {
                if(!visited[neighbor]){
                    visited[neighbor] = true;
                    queue.push(neighbor);
                }
            });
        }
        return result;
    }
```
