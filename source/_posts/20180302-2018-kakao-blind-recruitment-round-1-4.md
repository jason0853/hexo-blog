---
title: 2018 카카오 코딩 테스트 (셔틀버스)
tags:
  - Algorithm
  - Javascript
thumbnail:
  - ../../../../images/algorithm/2018-kakao-blind-recruitment-logo.png
categories:
  - Algorithm
date: 2018-03-02 23:15:27
---


![](../../../../images/algorithm/2018-kakao-blind-recruitment-logo.png)

이번 문제는 정말 어려웠었습니다. 난이도도 중으로 표시되어 있어서 쫄았었는데 진짜 푸는데 몇 일 걸렸습니다. 예상컨대 다음 문제부터는 못 풀지도 모르겠습니다. ㅠㅠ 하이튼 같이 한번 제가 푼 방식을 공유해보겠습니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-4-01.png)

* 셔틀의 시작 시간 - 09:00
* n회 t분 간격으로 도착하므로 예를 들어 5회 10분 간격이면 09:00, 09:10, 09:20, 09:30, 09:40 이렇게 셔틀버스가 운영됩니다.
* 셔틀버스 도착 시간에 딱 맞춰서 줄을 서도 빈자리가 있으면 탈 수 있습니다.
* 같은 시각에 온 사람일경우 주인공 콘은 맨 뒤에 서야합니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-4-02.png)

입출력 형식 및 예제를 다 살펴보았습니다. 제일 늦은 시각을 출력해보겠습니다.

``` js
const test = [
  { n: 1, t: 1, m: 5, timetable: ["08:00", "08:01", "08:02", "08:03"] },
  { n: 2, t: 10, m: 2, timetable: ["09:10", "09:09", "08:00"] },
  { n: 2, t: 1, m: 2, timetable: ["09:00", "09:00", "09:00", "09:00"] },
  { n: 1, t: 1, m: 5, timetable: ["00:01", "00:01", "00:01", "00:01", "00:01"] },
  { n: 1, t: 1, m: 1, timetable: ["23:59"] },
  { n: 10, t: 60, m: 45, timetable: ["23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59"] },
];

function shuttleBus(n, t, m, timetable) {
  function calculateTime(str) {
    const hour = Number(str.substr(0, 2)) * 60,
          min = Number(str.substr(3, 2)) * 1;
    return hour + min;
  }
  
  function formatTime(num) {
    const hh = String(Math.floor(num / 60)).padStart(2, '0'),
          mm = String(num % 60).padStart(2, '0');
    return `${hh}:${mm}`;
  }

  const startTime = calculateTime('09:00'),
        lastTime = startTime + t * (n - 1),
        crewTime = timetable.map(calculateTime).filter(time => time <= lastTime).sort();

  for (let i = 0; i < n; i++) {
    const shuttleTime = startTime + i * t,
          crewNum = crewTime.filter(time => time <= shuttleTime).length;

    if (i === n - 1) {
      if (crewNum < m) return formatTime(lastTime);
      return formatTime(crewTime[m - 1] - 1);
    } else {
      if (crewNum > m) {
        crewTime.splice(0, m);
      } else {
        crewTime.splice(0, crewNum);
      }
    }
  }
}

for (let obj of test) {
  console.log(shuttleBus(obj.n, obj.t, obj.m, obj.timetable));
}

// output: 09:00, 09:09, 08:59, 00:00, 09:00, 18:00
```

* <code>calculateTime</code> 메서드는 string 타입으로 된 시간 표시를 number 타입으로 계산하는 함수입니다.
***예) 시간 X 60 / 분 X 1 - 09:01 - 541***
* <code>formatTime</code> 메서드는 <code>calculateTime</code>와 반대로 다시 string 타입으로 시간 표시를 해주는 함수힙니다. 마지막 값을 리턴해줄때 사용합니다.
* <code>startTime + t \* (n - 1)</code> 계산값은 버스의 마지막 시간을 구하게 됩니다.
***예) 셔틀버스 시작 시간 + 시간 간격 X (셔틀버스 횟수 - 1)***
* <code>timetable.map(calculateTime)</code> 배열 값을 전부 time 계산값으로 바꿔줍니다.
* <code>timetable.filter(time => time <= lastTime)</code> 셔틀버스의 마지막 시간보다 늦게 오면 탈 수 없으므로 필터링 해줍니다.
* <code>timetable.sort()</code> 배열을 오름차순으로 정렬해줍니다.
* <code>n</code> 버스 횟수만큼 for loop를 돌려서 각각의 <code>shuttleTime</code>(셔틀버스 도착시간)과 <code>crewNum</code>(셔틀버스 도착시간 전에 온 크루들 몇 명)를 구합니다.
* <code>splice</code> 함수를 통해 셔틀버스 도착시간 전에 온 크루들이 버스 사이즈보다 더 많을 경우 다 타지 못하므로 사이즈(<code>m</code>)만큼 배열을 잘라줍니다.
* <code>if (i === n - 1)</code> 조건문을 통해 마지막 loop를 돌때 crewTime의 남아있는 배열 시간값을 알수 있습니다. 이 값들을 통해 버스를 탈 수 있는 가장 늦은 시각을 구할 수 있습니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-4-03.png)

문제 해설을 읽어보니 역시나 어려운 문제였던것 같습니다. 정답률이 26.79%네요. 이번 문제 같은 경우 마지막 버스 시간까지 꼼꼼히 고려하지 않으면 실수할 경우가 많았을 것 같습니다.

### Wrap-up

이 문제 같은 경우는 같이 스터디하는 친구가 힌트를 주면서 풀었기 때문에 그나마 접근해서 풀 수 있었습니다. 결국 풀다보니 코드도 동일해져버렸지만 이런 문제를 풀면서 다양하게 접근하는 방식을 익힐 수 있어서 좋은 시간이었던 것 같습니다. 몇 일 걸렸지만 ㅠㅠ 혹시 이거 말고 다른 로직으로 푸신분 공유해주시면 정말 감사하겠습니다.

### Reference

[카카오톡 문제 및 해설](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)