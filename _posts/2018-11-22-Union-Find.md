---
layout: post
title: "Union-Find Algorithm"
author: "Jaeik Lee"
tags: [algorithm, union, find, union-find]
desc: union-find description
---

# Union-Find Algorithm

---

## **Union-Find 란 ?**
```
# 공통 원소가 없는, "상호 배타적" 부분 집합을 저장하는 자료 구조
# Union(합치기), Find(찾기) 연산을 지원해야하기 떄문에 Union-Find Algorithm이라 불림
# 서로소 집합(Disjoint Set), 병합 찾기 집합(Merge Find Set) 이라고도 불린다.

# Union 연산    : 두 원소 a, b가 주어질 때, 이들이 속한 두 집합을 하나로 합친다.  
# Find 연산     : 어떤 원소 a가 주어질 때, 이 원소가 속한 집합을 반환한다.
```

---

## **Union-Find 예시**
```
# 아래와 같이 원소들이 있고,
{ 1 }   { 2 }   { 3 }   { 4 }   { 5 }   { 6 }

# 아래와 같이 원소들의 관계를 입력 받는다면
(1,2) (2,5) (5,1) (3,4) (4,6)

# 이와 같이 2개의 서로소 집합이 나온다.
{ 1 2 5 }   { 3 4 6 }
```

---

## **Union-Find 구현**
```
# 초기화, Union, Find 3가지로 구분된다.
```

```c++
//  초기화
//  원소들간 연결된 정보가 없기 때문에 부모로 자기 자신을 가진다.
for( int i=1 ; i<=n ; i++ ) {
    root[i] = i;
}
```

```c++
//  Find
//  최상단 노드인 root 노드를 반환
//  이것은 x가 속한 집합의 id를 반환한다고 이해하면 된다.
int find(int x) {

    //  초기화를 root[x] = x; 로 했기 때문에
    //  root[x] = x의 경우는 x가 root인 경우이다.
    //  따라서, 현재 속한 집합 id를 반환
    if( root[x] == x ) {
        return x;
    } else {
        //  root가 아니면, root를 찾아간다,
        //  { 1 } <-- { 2 } ... <-- { 100 } 과 같은 최악 고려 X
        //  시간복잡도 : O(N)
        //  최적화 가능
        return find(root[x]);

        //  경로 압축
        //  시간 복잡도 : O(logN)
        //  find의 결과 값을 모두 root[x]로 초기화
        root[x] = find(root[x]);
        return root[x];
    }
}
```
```c++
//  Union
//  각 원소가 속한 root 값을 찾는다.
//  y의 root를 x의 root로 초기화, 순서 상관 X
void union(int x, int y) {
    int xRoot, yRoot;
    xRoot = find(x);
    yRoot = find(y);

    root[yRoot] = xRoot;
}

//  Union 최적화
//  union-by-rank
//  rank에 트리의 높이를 저장
//  항상 높이가 더 낮은 트리를 높은 트리 밑에 넣는다.
int root[MAX_SIZE];
int rank[MAX_SIZE];
for(int i=0 ; i<MAX_SIZE ; i++) {
    root[i] = i;
    rank[i] = 0;    //  트리의 높이가 처음에는 0
}

void union(int x, int y) {
    int xRoot, yRoot;
    xRoot = find(x);
    yRoot = find(y);

    //  root값이 같으면 이미 같은 트리,
    //  따라서 합치지 않는다.
    //  무한루프에 빠질 수 있음
    if( xRoot == yRoot ) return;

    //  union-by-rank 최적화
    //  높이가 더 낮은 트리를 높이가 더 높은 트리 밑에 넣는다.
    //  왜지 ? 이건 찾아봐야할듯.
    if( rank[x] > rank[y] ) {
        root[yRoot] = root[xRoot];
    } else if( rank[x] == rank[y] ) {
        //  다른 집합이지만, 트리의 높이가 같은 경우
        //  트리를 합치고, x의 높이를 + 1
        rank[yRoot] = xRoot;
        rank[xRoot]++;
    } else {
        root[xRoot] = root[yRoot];
    }
}

```
```c++
//  트리에 속한 노드의 수 구하기
int nodeCount[MAX_SIZE];
for( int i=0 ; i<MAX_SIZE ; i++ ) {
    nodeCount[i] = 1;
}

int union(int x, int y) {
    int xRoot, yRoot;
    xRoot = find(x);
    yRoot = find(y);
    
    root[yRoot] = xRoot;
    nodeCount[xRoot] += nodeCount[yRoot];
    nodeCount[yRoot] = 1;

    return nodeCount[xRoot];
}
```

---

### ***References***
```
# http://bowbowbow.tistory.com/26
# https://twpower.github.io/115-union-find-disjoint-set
# https://gmlwjd9405.github.io/2018/08/31/algorithm-union-find.html
```