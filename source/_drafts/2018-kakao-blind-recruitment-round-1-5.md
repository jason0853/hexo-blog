---
title: 2018-kakao-blind-recruitment-round-1-5
tags:
  - Algorithm
  - Javascript
thumbnail:
  - ../../../../images/algorithm/algorithm-logo.png
categories:
  - Algorithm
---


![](../../../../images/algorithm/algorithm-logo.png)

이번 알고리즘은 중으로 표시되어있지만 생각보다 쉬웠습니다. 긴말 할 필요없이 바로 문제 요약해보고 풀이해보겠습니다.

문제를 읽어보니 **자카드 유사도**란 낯선 단어가 보입니다. 그런데 겁먹을 필요가 없습니다. 문제안에서 자카도 유사도에 대해 상세히 설명해줍니다. **두집합 사이에 교집합 크기를  합집합 크기로 나눈값**이라고 정의합니다. 이해가 바로 되니 길게 설명할 필요도 없어서 너무 좋습니다.ㅋㅋ 이번엔 입출력 형식을 한번 살펴보고 정리해보겠습니다.


* 2개의 문자열이 들어옵니다.
* 각 문자열을 두 글자씩 잘라내야합니다.
* 두 글자씩 끊을 때 공백, 특수문자, 숫자가 함께 있는 경우는 버립니다.
* 대소문자는 구별은 동일한 걸로 취급합니다.
* 자카드 유사도 = 교집합 / 합집합 * 65536
* 교집합이 공집합일 경우 1로 간주합니다.
* 소수점 이하는 버리고 정수부분만 출력합니다.

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
* 변수 <code>flag</code>는 각각의 배열에 push하기 위해 바깥 <code>for</code>구문에서 <code>flag = true</code>로 변경합니다.
* <code>indexOf()</code>함수를 이용해 두 배열 사이의 유사한 문자가 있으면 -1보다 큰 숫자가 반환됩니다. 조건문이 참이라면 <code>intSecNum</code> 교집합 크기값을 증가시킵니다.
* 교집합이 없을 경우도 있으므로 <code>if (!intSecNum) intSecNum = 1;</code> 예외 처리를 해줍니다.
* <code>arr1.length + arr2.length - intSecNum</code> - 두 배열 합에서 교집합을 빼주면 합집합의 크기(<code>unionNum</code>)가 됩니다.
* 마지막 입출력 예제를 보면 합집합이 공집합일 경우(두 배열의 합이 0일 경우)도 있는데 <code>Math.abs()</code> 함수를 사용하지 않으면 마이너스 값이 출력되기 때문에 이 부분에 유의하셔야합니다.
* <code>Math.floor(intSecNum / unionNum * 65536)</code> 위 문제에서 말한대로 자카드 유사도 식에 65536 곱해서 소숫점 이하는 버리고 정수부분만 반환시킵니다.


문제 해설을 보니 정답률이 [지난번 셔틀버스](https://jason0853.github.io/2018/03/02/2018-kakao-blind-recruitment-round-1-4/)보다 높게 나왔습니다. 문제랑 입출력 예제만 꼼꼼히 잘 살펴본다면 실수하지 않고 다들 푸실 수 있을 것 같습니다.

### Wrap-up

개인적으로 지금까지 풀어본 알고리즘 문제들 중에 가장 쉬웠던 것 같습니다. 풀기 전에 머릿속으로 '이런 방식으로 짜야지' 생각하고 풀었는데 술술 잘 풀려서 기분이 좋았습니다. 그럼 다음 포스팅에서 찾아뵙겠습니다.

### Reference

[카카오톡 문제 및 해설](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)