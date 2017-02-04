---
layout: post
title: "Sorting Algorithm(Quick, Merge, Bucket, Insertion)"
description: "정렬 알고리즘(퀵 정렬, 병합 정렬, 버킷 정렬, 삽입 정렬)"
date: 2017-02-04
tags: [algorithm, sort]
comments: true
share: true
---

## 비교, 치환 함수
각 정렬 알고리즘에 대해 알아보기 전에 필요한 함수들을 추가해보도록 하겠습니다.

```cpp
bool cmp(int a, int b){
	return a < b;
}
```
대소 비교를 위한 cmp 함수입니다. 다른 비교를 원할 시 함수 내부를 수정하면 됩니다.


```cpp
void swap(int &a, int &b){
	int temp = a;
	a = b;
	b = temp;
}
```

두 값이 들어왔을 때 바꾸어 주기 위한 swap함수입니다.

### 삽입 정렬(Insertion Sort)
배열의 요소들을 처음부터 정렬해 나아가는 방식입니다. 

시간복잡도: __O(n^2)__

```cpp
void sortI(int *a, int s, int e){
	for(int i=s; i<e; i++){
		int temp = a[i];
		int j = 0;
		for(j=i-1; j>=s && cmp(temp, a[j]); j--){
			a[j+1] = a[j];
		}
		a[j+1] = temp;
	}
}
```

```cpp
sortI(d, 0, n);
```


### 퀵 정렬(Quick Sort)
기준(pivot)을 정한 후 pivot을 기준으로 큰 원소와 작은 원소들로 나누어 가면서 정렬해 나아가는 방식입니다. pivot을 잘 정하는 것이 알고리즘의 성능에 영향을 미치며, 정렬할 원소들이 작거나 pivot이 비효율적으로 정해질 경우에는 성능이 떨어집니다.

시간복잡도: __O(n^2)__ (평균적인 경우: __O(nlogn)__)

```cpp
void sortQ(int *d, int l, int r){
	int i = 1;
	int j = r;
	int mid = d[(l+r)/2];
	while(i<=j){
		while(cmp(d[i], mid)) i++;//d[i]<mid
		while(cmp(mid, d[j])) j--;//mid<d[j]
		if(i<=j){
			swap(d[i++], d[j--]);
		}
	}
	if(i<r) sortQ(d, i, r);
	if(l<j) sortQ(d, l, j);
}
```


```cpp
sortQ(d, 0, n-1);
```

### 병합 정렬(Merge Sort)
전체 원소를 반으로 계속 나눈 후 이를 병합하는 과정에서 정렬을 수행합니다. 분할정복(Divide and Conquer) 방식을 기반으로 구현됩니다.

시간복잡도: __O(nlogn)__

```cpp
void sortM(int *d, int len){
	if(len<=1) return;
	int mid = len/2;
	int i = 0;
	int j = mid;
	int k = 0;
	int *buf = new int[len];
	sortM(d, mid);
	sortM(d+mid, len-mid);
	while(i<mid && j<len) buf[k++] = (d[i] < d[j] ? d[i++] : d[j++]);
	while(i<mid) buf[k++] = d[i++];
	while(j<len) buf[k++] = d[j++];
	for(int i=0; i<len; i++) d[i] = buf[i];
	delete[] buf;
}
```


```cpp
sortM(d, n);
```

### 버킷 정렬(Bucket Sort), 카운팅 정렬(Counting Sort)
배열에 포함되는 숫자들을 기준으로 개수를 세어서 정렬하는 방식입니다. 배열 내 값의 크기가 작을 경우에는 한번에 개수들을 세어서 정렬하면 되지만(Counting Sort), 클 경우에는 버킷(Bucket)을 나누어서 정렬을 여러번 수행해 주면 됩니다. 

시간복잡도: __O(n)__

```cpp
void sortC(int *d, int len){
	int max = d[0];
	for(int i=1; i<len; i++){
		if(max < d[i]) max = d[i];
	}
	int exp = 1;
	int *tmp = new int[len];
	while(max/exp > 0){
		int cnt[10];
		memset(cnt, 0, sizeof(cnt));
		for(int i=0; i<len; i++){
			cnt[d[i]/exp%10]++;
		}
		for(int i=1; i<len; i++){
			cnt[i] += cnt[i-1];
		}
		for(int i=len-1; i>=0; i--){
			tmp[--cnt[d[i]/exp%10]] = d[i];
		}
		for(int i=0; i<len; i++){
			d[i] = tmp[i];
		}
		exp *= 10;
	}
}
```


```cpp
sortC(d, n);
```
