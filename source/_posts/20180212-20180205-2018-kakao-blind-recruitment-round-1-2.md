---
title: 2018 카카오 코딩 테스트 (다트 게임)
tags:
  - Algorithm
  - Javascript
thumbnail:
  - ../../../../images/algorithm/algorithm-logo.png
categories:
  - Algorithm
date: 2018-02-12 17:57:13
---


![](../../../../images/algorithm/algorithm-logo.png)

[지난 포스팅](https://jason0853.github.io/2018/02/05/2018-kakao-blind-recruitment-round-1-1/)에 이어 신입 공채 두번째 문제를 풀어보도록 하겠습니다. 지난번 문제보다는 살짝 어렵지만 너무 겁먹지 마세요. 막상 풀면 별거 아닌것 같습니다.ㅋㅋ

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-2-01.png)

문제가 길어서 이해하는데 지난번보다 오래 걸렸습니다. 위 문제 내용을 한번 요약해보겠습니다.

* 총 기회는 3번
* 점수는 0-10점까지 (10점 두 자릿수이기 때문에 유의!)
* S - 제곱, D - 2제곱, T - 3제곱
* 스타상(*) - 2배, 해당 점수 및 이전 점수 2배, 스타상끼리 중첩시 4배
* 아차상(#) - 해당 점수 마이너스, 스타상과 중첩시 마이너스 2배

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-2-02.png)

입출력 예제를 참고해서 똑같은 answer를 도출해보겠습니다.

``` js
const dartResult = ['1S2D*3T', '1D2S#10S', '1D2S0T', '1S*2T*3S', '1D#2S*3S', '1T2D3D#', '1D2S3T*'];

function dart(point) {
  const scores = point.match(/\d+/g),
        bonus = point.match(/[SDT]/g),
        bonusOpt = point.match(/[SDT][#\*]?/g),
        temp = [];

  let result = 0;

  for (let i = 0; i < 3; i++) {
    const score = Number(scores[i]);
          opt = bonusOpt[i].replace(/[SDT][\*]/g, '*').replace(/[SDT][#]/g, '#').replace(/[SDT]/g, null);

    switch (bonus[i]) {
      case 'D': temp.push(Math.pow(score, 2)); break;
      case 'T': temp.push(Math.pow(score, 3)); break;
      default: temp.push(score);
    }

    switch (opt) {
      case '*':
        if (i === 2) {
          temp.map((score, idx) => {
            if (idx !== 0) temp[idx] = score * 2
          });
        } else temp.map((score, idx) => temp[idx] = score * 2);
        break;
      case '#': temp[i] = temp[i] * -1;
    }
  }

  for (let i = 0; i < 3; i++) result += temp[i];

  return result;
}

for (let point of dartResult) {
  console.log(dart(point));
}

// output: 37, 9, 3, 23, 5, -4, 59
```

* 숫자(1-10), 영문(SDT), 특수문자(#/\*)를 <code>match()</code> 메서드를 이용하여 각 문자열을 배열로 반환해서 각 변수에 저장해두었습니다.
***정규식 <code>\d+</code> 통하여 두자릿수 숫자 10을 해결하였습니다.***
* 보너스와 옵션을 묶어서 <code>bonusOpt</code> 변수를 따로 둔 이유는 특수문자만 정규식을 사용하여 처리할 경우 스타상(\*)과 아차상(#)을 몇번째 점수에 처리해야될지 미지수이기 때문입니다.
* <code>replace()</code> 메서드를 이용하여 스타상과 있을 경우(\*), 아차상이 있을 경우(#), 둘다 없을 경우(null) 값으로 처리하여 배열로 반환합니다.
* <code>Math.pow()</code> 메서드를 이용하여 점수와 보너스 거듭 제곱 연산처리를 해줍니다. 문자열 'S'는 1제곱이므로 따로 할 필요가 없습니다.
* 스타상(\*)일 경우 조건문을 통해 해당 점수의 이전 점수까지만 2배 처리를 해줍니다.
***조건문이 추가되지 않고 세번째 점수에 아차상이 존재할 경우 모든 점수에 2배 점수가 적용됩니다. 이 부분에 유의해주세요.***

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-2-03.png)

문제 해설을 보니 정규식을 이용하여 문자열 처리를 잘 활용할 수 있는지 묻는 문제였습니다.

### Wrap-up

결국 문제는 풀었지만 최적화에 대해 좀 더 고민을 해야될 것 같습니다. 여유가 생기면 다시 한번 고민해서 풀어보고 포스팅 해보겠습니다. 그리고 이번 다트 게임 문제를 통하여 정규식 공부를 열심히 해야겠다는 생각을 하게 되었습니다. 매번 구글링을 통해 copy/paste 했던 제 자신이 한심해지네요.ㅠㅠ 위 코드에 잘못된 부분이나 좀 더 나은 로직을 공유해주실 분은 댓글 부탁드립니다.

### Reference

[카카오톡 문제 및 해설](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)