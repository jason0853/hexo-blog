---
title: 2018 카카오 코딩 테스트 (비밀 지도)
tags:
  - Algorithm
  - Javascript
thumbnail:
  - ../../../../images/algorithm/algorithm-logo.png
categories:
  - Algorithm
date: 2018-02-05 13:41:22
---



![](../../../../images/algorithm/algorithm-logo.png)

최근 카카오에서 신입 공채로 코딩 테스트를 진행했었습니다. 문제들을 보니 재미있을 것 같아서 한번 풀어보고 블로그에 포스팅 해보기로 하였습니다.
우선 1번 문제인 비밀지도(난이도: 하) 문제부터 차례대로 포스팅해보겠습니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-1-01.png)

그림과 문제를 보고 저 나름대로 짧게 다시 요약해보겠습니다.

* 첫번째 지도 + 두번째 지도 = 세번째 지도
* \# = 1, 공백 = 0
* 두 지도와 2진수를 각각 비교해보겠습니다. 예를 들어 빨강색으로 표시된 부분만 한 번 비교해보겠습니다.

``` plain
// 지도 비교
###
# # #
-----
### #

// 이진수 비교
11100
10101
-----
11101
```

***결국 이진수 11101은 ###공백#입니다.***

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-1-02.png)

입출력 예제에서 첫번째 매개변수 값과 두번째 매개변수 값을 가지고 출력이 똑같이 나오도록 하겠습니다.

```js
function addZero(digits, data) {
  const zero = '0';
  
  if (data.length !== digits) {
    for (let i = 0, len = digits - data.length; i < len; i++) {
      data = zero + data;
    }
  }

  return data;
}

function secretMap(n, arr1, arr2) {
  const result = [];
  let testNum1, testNum2;
  
  for (let i = 0; i < n; i++) {
    testNum1 = arr1[i].toString(2);
    testNum2 = arr2[i].toString(2);
    
    testNum1 = addZero(n, testNum1);
    testNum2 = addZero(n, testNum2);

    let chars = '';

    for (let i = 0; i < n; i++) {
      if (Number(testNum1[i]) || Number(testNum2[i])) chars += '#';
      else chars += ' ';
    }

    result.push(chars);
  }
  
  return result;
}

secretMap(5, [9, 20, 28, 18, 11], [30, 1, 21, 17, 28]);
secretMap(6, [46, 33, 33 ,22, 31, 50], [27 ,56, 19, 14, 14, 10]);

// output1 : ["#####","# # #", "### #", "# ##", "#####"]
// output2 : ["######", "### #", "## ##", " #### ", " #####", "### # "]
```

* <code>toString(2)</code> 메서드를 통해 10진수를 2진수로 변경합니다.
* <code>addZero</code> 메서드를 통해 0을 앞에 추가하면서 자릿수를 맞춰줍니다.
* <code>||</code> OR 연산자를 통해 하나라도 참이면 '#', 둘 다 거짓이면 공백 처리를 해줍니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-1-03.png)

정답은 맞혔지만 위 문제 해설을 보면은 **비트 연산**을 잘 활용할 수 있는지를 묻는 문제였습니다. 그럼 **비트 연산**을 통해 다시 풀어보겠습니다.

``` js
function secretMap(n, arr1, arr2) {
  const result = [];

  for (let i = 0, int, binary; i < n; i++) {
    int = (arr1[i] | arr2[i]);
    binary = int.toString(2);
    result.push(binary.replace(/1/g, '#').replace(/0/g, ' '));
  }

  return result;
}

secretMap(5, [9, 20, 28, 18, 11], [30, 1, 21, 17, 28]);
secretMap(6, [46, 33, 33 ,22, 31, 50], [27 ,56, 19, 14, 14, 10]);

// output1 : ["#####","# # #", "### #", "# ##", "#####"]
// output2 : ["######", "### #", "## ##", " #### ", " #####", "### # "]
```

* <code>|</code> **비트 연산 OR**을 사용하면 연산 자체는 2진수로 하지만 반환값은 자바스크립트 표준값으로 나옵니다.
예) 9 | 30 = 31
* <code>toString(2)</code>을 통해 2진수 값으로 변환합니다.
* <code>replace()</code> 메서드를 통해 2진수 문자열을 1일 때 '#', 0일 때 공백 처리를 해줍니다. 첫번째 인자에 있는 플래그(Flag) <code>g</code>는 문자열 전체를 검색해줍니다.

### Wrap-up

블로그를 포스팅하면서 첫번째 알고리즘을 다뤄봤습니다. **비트 연산**을 묻는 의도인 것을 파악하지 않고 풀었을 경우와 알고 풀었을 경우의 코드를 비교해보니 코드량이 1/3로 줄었습니다. 포스팅을 보신 뒤 잘못된 부분이나 좀 더 최적화 코드를 공유해주실 분은 댓글 부탁드립니다.

### Reference

[카카오톡 문제 및 해설](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)