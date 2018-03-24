---
title: 추석 트래픽
tags:
  - Algorithm
  - Javascript
thumbnail:
  - ../../../../images/algorithm/2018-kakao-blind-recruitment-logo.png
categories:
  - Algorithm
---

![](../../../../images/algorithm/2018-kakao-blind-recruitment-logo.png)


드디어 카카오톡 신입 공채 1차 코딩 테스트 마지막 문제입니다. 마지막 문제라 어려울거라 예상했는데 역시나 어려웠습니다.ㅠㅠ

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-7-01.png)

문제를 살짝 읽어보니 로그 데이터를 분석한 후 초당 최대처리량을 구하는 문제입니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-7-02.png)
![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-7-03.png)
![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-7-04.png)

* 처리시간은 시작시간과 끝시간을 포함해야합니다.
* 요청 갯수를 카운트 할때 요청 갯수가 변경되는 순간은 1초(1000ms)입니다.

``` js
const test = [
  ["2016-09-15 01:00:04.001 2.0s", "2016-09-15 01:00:07.000 2s"],
  ["2016-09-15 01:00:04.002 2.0s", "2016-09-15 01:00:07.000 2s"],
  ["2016-09-15 20:59:57.421 0.351s", "2016-09-15 20:59:58.233 1.181s", "2016-09-15 20:59:58.299 0.8s", "2016-09-15 20:59:58.688 1.041s", "2016-09-15 20:59:59.591 1.412s", "2016-09-15 21:00:00.464 1.466s", "2016-09-15 21:00:00.741 1.581s", "2016-09-15 21:00:00.748 2.31s", "2016-09-15 21:00:00.966 0.381s", "2016-09-15 21:00:02.066 2.62s"]
];

function chuseokTraffic(logs) {
  const numArr = logs.map(log => log.match(/\d+/g).slice(3, 9).map(c => Number(c)));  // 숫자 정규식 / 날짜 제외 slice / number 타입으로 변경
  const time = [], lengths = [];
  let endTime, interval, empty = [];
  for (let arr of numArr) {
    arr.forEach((elem, idx) => {
      switch (idx) {
        case 0: endTime = 1000 * 60 * 60 * elem; break;
        case 1: endTime += 1000 * 60 * elem; break;
        case 2: endTime += 1000 * elem; break;
        case 3: endTime += elem; break;
        case 4: interval = elem * 1000; break;
        case 5: interval = elem ? interval + elem : interval; break;
      }
    });

    time.push({ startTime: endTime - interval + 1, endTime });
  }

  time.forEach(t => {
    empty = empty.filter(end => end > t.startTime - 1000);
    empty.push(t.endTime);
    lengths.push(empty.length);
  });

  return Math.max(...lengths);
}

for (let arr of test) {
  console.log(chuseokTraffic(arr));
}

// output: 1, 2, 7
```

* 로그데이터를 처리하는데 필요한 부분만 필터링을 해서 <code>numArr</code> 변수에 저장합니다.
* 1s = 1,000ms, 1m = 60s * 1,000ms, 1h = 60m * 60,000ms를 참고하여 시간을 계산해줍니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-7-05.png)

확실히 정답률이 낮았습니다. 다른 문제들과 달리 접근자체를 하지 못했었습니다. 문제해설을 보시면 **o(n log n)**으로 풀 수 있는 방법도 있다고 합니다. 다음에 기회가 된다면 이런 방식으로 한 번 고민해서 풀어봐야겠습니다.

### Wrap-up

마지막 문제까지 다 풀어보았습니다. 다음에 시간이 허락된다면 풀었던 문제들을 좀 더 개선된 알고리즘으로 해결해서 공유해볼 수 있도록 노력해보겠습니다.

### Reference

[카카오톡 문제 및 해설](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)