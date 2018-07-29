---
layout: post
title: "Hash Table"
author: "younari"
---

# 해시 테이블

### 해시 함수란?
- 문자열(바이트열)을 받아서 숫자를 반환하는 함수
- 예) “apple” → 3
- 배열의 3번째 인덱스에 “apple”을 저장!

### 좋은 해시 함수란?
- 배열에 값을 골고루 분포 시키는 함수

### 해시함수와 배열을 합치면 해시 테이블이 된다.
- 배열과 리스트는 직접 메모리를 할당하지만 
- 해시 테이블은 **해시함수를 통해 더 총명하게 어디에 원소를 저장할 지 결정**할 수 있다
- Key : Value



# 예시 상황

### 웹 주소와 IP 주소의 맵핑에도 해시 테이블 기능이 사용된다.
- 예) “baidu.com” → 10.172 ~~

### 웹 페이지의 캐싱도 해시 테이블에 저장된다
- 서버가 추가 탐색 작업 없이 해시 테이블에 저장된 자료를 바로 전달할 수 있다.
- 예) “특정 URL” → 웹 페이지 자료 

# 유용한 상황
- 어떤 것을 다른 것과 연관시켜야 할 때 그 관계를 모형화 할 수 있다
- 무언가를 찾아 보아야 할 때 시간을 단축할 수 있다
- 중복된 항목을 방지해야 할 때 (같은 키 값이라면 항상 같은 인덱스를 반환)


### 탐색이 필요한 상황에서 탐색하는 방법과 효율성 (평균적으로)
- **단순 탐색**: 하나씩 탐색할 경우 O(n) 시간
- **이진 탐색**: O(logN) 시간
- **해시 테이블**: O(1) 시간 (상수 시간)


<br>
<br>
<hr>




# 배열과 연결리스트
- 컴퓨터의 메모리는 거대한 서랍장과 같다.
- 여러개의 항목을 저장하고 싶을 때는 배열이나 리스트를 사용할 수 있다.
- 배열을 쓰면 모든 항목은 이웃하는 위치에 저장된다.
- 리스트를 쓰면 모든 항목이 흩어지지만, 각 항목은 다음 항목의 주소를 저장하고 있다.
- 배열은 읽기가 빠르다.
- 연결리스트는 삽입과 삭제가 빠르다.
- 배열의 모든 원소는 **같은 자료형**이어야 한다.
- 연결리스트는 데이터 덩어리(노드 Node)를 저장할 때 **그 다음 순서의 자료가 있는 위치**를 데이터에 포함시키는 방식으로 자료를 저장한다.
- 배열에 비해 데이터의 추가/삽입 및 삭제가 용이하나 **순차적으로 탐색하지 않으면 특정 위치의 요소에 접근할 수 없어** 일반적으로 탐색 속도가 떨어진다.
- 탐색 또는 정렬을 자주 하면 배열을, 추가/삭제가 많으면 연결 리스트를 사용하는 것이 유리하다. 
- 배열은 쓰지않는 공간까지 전부 예약해두고 있어야 하기 때문에 공간 낭비가 생긴다.

# Hash Table
- [스위프트 알고리즘 클럽](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Hash%20Table)
- **[ Key : Value ]**
- 해시테이블은 **해시함수와 배열을 결합해서** 만든다.
- 대부분의 프로그래밍 언어는 해시테이블을 지원한다.
- 딕셔너리, 연관 배열, maps
- 어떤 것과 다른 것 사이의 관계를 모형화하는 것
- 중복을 막을 수 있다.
- 서버에게 작업을 시키지 않고, 자료를 캐싱할 수 있다.
- 충돌을 줄이는 해시 함수가 좋은 함수이다.
- 사용률이 0.7보다 커지면 해시 테이블을 리사이징 한다.
- 평균적인 경우 해시 테이블의 성능은 상수 시간이다. O(1)
- It is difficult to predict where in the array your objects end up. Hence, dictionaries **do not guarantee any particular order of the elements in the hash table.**

<br>
<br>
<br>
<br>
<hr>

*written in Shenzhen, @Muji Hotel, 2018.04.08*