---
title: 2018 카카오 코딩 테스트 (캐시)
tags:
  - Algorithm
  - Javascript
thumbnail:
  - ../../../../images/algorithm/algorithm-logo.png
categories:
  - Algorithm
date: 2018-02-18 16:52:13
---


![](../../../../images/algorithm/algorithm-logo.png)

이번 문제는 난이도가 [지난 포스팅](https://jason0853.github.io/2018/02/12/20180205-2018-kakao-blind-recruitment-round-1-2/)보다 쉬워서 조금 빨리 풀 수 있었습니다. 한번 같이 문제를 살펴볼까요?ㅋㅋ

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-3-01.png)

우선 제목에서도 볼 수 있듯이 캐시와 관련된 문제입니다. 캐시는 실제 개발할 때도 성능 개선을 하는데 있어서 중요한 부분을 차지하기 때문에 개발자들이 한번씩 같이 풀어보면 좋은 문제라고 생각합니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-3-02.png)

문제를 풀기 전에 주의할 점이나 모르는 용어들을 한 번 정리해보겠습니다.

* 대소문자를 구분하지 않는 점.
* **LRU(Least Recently Used)** - cache(임시 저장소)에 저장시킬때 공간이 부족하여 오래된 데이터를 퇴출시키고 새로운 데이터로 교체하는 가장 효율이 좋은 알고리즘.
* **cache hit** - 캐시에 참조하려는 데이터가 존재할 때.
* **cache miss** - 캐시에 참조하려는 데이터가 존재하지 않을 때.

입출력 및 조건 등을 다 읽어보니 실행시간을 구해내야합니다. 그럼 한번 알고리즘을 구현해보겠습니다.

``` js
const test = [
  { cacheSize: 3, cities: ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "Jeju", "Pangyo", "Seoul", "NewYork", "LA"] },
  { cacheSize: 3, cities: ["Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul"] },
  { cacheSize: 2, cities: ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"] },
  { cacheSize: 5, cities: ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"] },
  { cacheSize: 2, cities: ["Jeju", "Pangyo", "NewYork", "newyork"] },
  { cacheSize: 0, cities: ["Jeju", "Pangyo", "Seoul", "NewYork", "LA"] }
];

function cache(cacheSize, cities) {
  const cache = [], HIT = 1, MISS = 5;
  let time = 0;

  if (cacheSize === 0) return MISS * cities.length;
  
  cities.forEach((val) => {
    const city = val.toLowerCase(),
          currentSize = cache.length,
          cacheIdx = cache.indexOf(city);

    if (cacheIdx > -1) {
      time += HIT;
      cache.splice(cacheIdx, 1);
      cache.push(city);
    } else {
      if (currentSize >= cacheSize) cache.shift();
      time += MISS;
      cache.push(city);
    }
  });

  return time;
}

for (let obj of test) {
  console.log(cache(obj.cacheSize, obj.cities));
}

// output: 50, 21, 60, 52, 16, 25
```

* <code>cahceSize === 0</code>일 때는 <code>cities</code>를 Loop 처리해줄 필요가 없기 때문에 단순 연산을 한 뒤 return 시킵니다.
* 대소문자를 구분하지 않는다고 했으니 <code>toLowerCase()</code>를 이용하여 문자열을 소문자로 바꿔 <code>city</code>라는 변수에 할당시킵니다.
* <code>cache.indexOf(city)</code>를 실행시키면 일치하는 값이 있을 경우 해당 배열 index 값을 반환시켜주고 일치하는 값이 없을 경우 -1을 반환시켜줍니다.

***주의) 입출력 예제에 없는 다른 유형의 예제도 고려해야함***

``` plain
// cache size가 3일 경우
["New York", "LA", "Las Vegas", "LA"]
| new york |  |  |
| new york | la |  |
| new york | la | las vegas |
| new york | las vegas | la |
```

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-3-03.png)

문제 해설을 읽어보니 정답률이 많이 낮았습니다. 아마도 다른 경우를 고려하지 않고 답을 도출했을 경우 많이 틀렸을 것으로 예상됩니다. 솔직히 저도 스터디하는 친구가 코드 리뷰를 해주지 않았다면 이전 코드를 가지고 포스팅 했을겁니다.ㅠㅠ 그러나 이번 문제를 통해 LRU 알고리즘을 알게 된 좋은 계기가 되었던 것 같습니다.

### Wrap-up

일주일에 한번씩 스터디를 진행하고 있는데 서로 짠 코드를 리뷰하는 것은 정말 많은 장점을 가진 것 같습니다. 미처 생각하지 못한 로직이나 성능, 그리고 코드 가독성 등 많은 것을 다른 동료한테 배울 수 있어서 너무 좋았습니다.

### Reference

[카카오톡 문제 및 해설](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)





