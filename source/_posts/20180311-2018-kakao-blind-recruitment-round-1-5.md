---
title: 2018 카카오 코딩 테스트 (뉴스 클러스터링)
tags:
  - Algorithm
  - Javascript
thumbnail:
  - ../../../../images/algorithm/algorithm-logo.png
categories:
  - Algorithm
date: 2018-03-11 16:57:09
---



![](../../../../images/algorithm/algorithm-logo.png)

이번 알고리즘은 중으로 표시되어있지만 생각보다 쉬웠습니다. 긴말 할 필요없이 바로 문제 요약해보고 풀이해보겠습니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-5-01.png)

문제를 읽어보니 **자카드 유사도**란 낯선 단어가 보입니다. 그런데 겁먹을 필요가 없습니다. 문제안에서 **자카드 유사도**에 대해 상세히 설명해줍니다. **두집합 사이에 교집합 크기를  합집합 크기로 나눈값**이라고 정의합니다. 이해가 바로 되니 길게 설명할 필요가 없습니다.ㅋㅋ 이번엔 입출력 형식을 한번 살펴보고 정리해보겠습니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-5-02.png)

* 2개의 문자열이 들어옵니다.
* 각 문자열을 두 글자씩 잘라내야합니다.
* 두 글자씩 끊을 때 공백, 특수문자, 숫자가 함께 있는 경우는 버립니다.
* 대소문자 구별은 동일한 걸로 취급합니다.
* 자카드 유사도 = 교집합 / 합집합 * 65536
* 교집합이 공집합일 경우 1로 간주합니다.
* 소수점 이하는 버리고 정수부분만 출력합니다.
***다중집합일경우 교집합은 min(최소값) 합집합은 max(최대값)으로 계산을 해야합니다. 제가 초반에 이 부분을 고려하지 않고 코드를 작성하다 실수를 했었습니다.ㅠㅠ***

이제 코드를 공개해보겠습니다.

``` js
const test = [
  { str1: 'FRANCE', str2: 'french' },
  { str1: 'handshake', str2: 'shake hands' },
  { str1: 'aa1+aa2', str2: 'AAAA12' },
  { str1: 'E=M*C^2', str2: 'e=m*c^2' }
];

function newsClustering(str1, str2) {
  const arr = [];

  for (let i = 0; i < arguments.length; i++) {
    const str = arguments[i].toLowerCase();
    arr[i] = [];

    for (let j = 0, len = str.length - 1; j < len; j++) {
      let reg = str.substr(j, 2).match(/^[a-z]*$/);

      if (reg) arr[i].push(reg.input);
    }
  }

  const set = new Set([...arr[0], ...arr[1]]);
  let size1, size2, intsecNum = 0, unionNum = 0;

  set.forEach(val => {
    size1 = arr[0].filter(char => char === val).length;
    size2 = arr[1].filter(char => char === val).length;

    intsecNum += Math.min(size1, size2);
    unionNum += Math.max(size1, size2);
  });

  if (!intsecNum) intsecNum = 1;
  if (!unionNum) unionNum = 1;

  return Math.floor(intsecNum / unionNum * 65536);
}

for (let obj of test) {
  console.log(newsClustering(obj.str1, obj.str2));
}
```

* 파라미터로 전달받은 각 문자열(<code>str1</code>,<code>str2</code>)을 <code>toLowerCase()</code> 메서드를 이용하여 소문자로 만들어주었습니다. 대문자로 만들어도 무방합니다.
* 변수 <code>reg</code>는 문자열을 두 글자씩 끊은 다음 정규식으로 영문자열일 경우 배열 형태로 반환되어 저장됩니다. 만약 영문자 이외의 문자나 공백이 함께 오는 경우는 null값으로 반환되어 저장됩니다. 그렇다면 배열 값이 존재할 때만 빈 배열에 push하면 됩니다.
* <code>Set</code> 객체를 이용하여 두 배열에서 존재하고 있는 유일한 값만을 저장합니다. <code>set</code>이란 변수에 담긴 배열을 이용하여 두 배열(arr[0], arr[1]) 안에 일치하는 문자열 갯수를 구합니다.
* <code>size1</code>, <code>size2</code> 두 변수를 <code>Math.min()</code>, <code>Math.min()</code>를 이용하여 최소값(교집합 갯수), 최대값(합집합 갯수)을 구합니다.
* 교집합과 합집합이 0일 경우도 있으므로 예외처리(1) 값을 처리해줍니다.
* <code>Math.floor(intSecNum / unionNum * 65536)</code> 위 문제에서 말한대로 자카드 유사도 식에 65536 곱해서 소숫점 이하는 버리고 정수부분만 반환시킵니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-5-03.png)

문제 해설을 보니 정답률이 [지난번 셔틀버스](https://jason0853.github.io/2018/03/02/2018-kakao-blind-recruitment-round-1-4/)보다 높게 나왔습니다. 문제랑 입출력 예제만 꼼꼼히 잘 살펴본다면 실수하지 않고 다들 푸실 수 있을 것 같습니다.

### Wrap-up

개인적으로 지금까지 풀어본 알고리즘 문제들 중에 가장 쉬웠던 것 같습니다. 풀기 전에 머릿속으로 '이런 방식으로 짜야지' 생각하고 풀었는데 술술 잘 풀려서 기분이 좋았습니다. 사실 초반에 빨리 푸는 바람에 실수했었습니다.ㅠㅠ 그럼 다음 포스팅에서 찾아뵙겠습니다.

### Reference

[카카오톡 문제 및 해설](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)