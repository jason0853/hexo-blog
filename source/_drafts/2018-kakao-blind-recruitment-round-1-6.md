---
title: 2018 카카오 코딩 테스트 (프렌즈4블록)
tags:
  - Algorithm
  - Javascript
thumbnail:
  - ../../../../images/algorithm/2018-kakao-blind-recruitment-logo.png
categories:
  - Algorithm
---

![](../../../../images/algorithm/2018-kakao-blind-recruitment-logo.png)

이번 알고리즘 문제는 그림을 보면 풀고 싶게 만드는 게임 관련 문제입니다. 문제를 한번 살펴보겠습니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-6-01.png)
![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-6-02.png)
![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-6-03.png)
![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-6-04.png)
![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-6-05.png)

문제를 읽어보면 같은 모양의 2X2형태 블록이 붙어 있을 경우 지워지면서 위에 남아 있는 블록들은 빈공간으로 이동합니다. 카카오톡에서 이런 게임들은 다들 한번씩은 해보셨을거라 다른 문제보다 한번에 이해가 되셨을겁니다.ㅎㅎ

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-6-06.png)

문제를 풀기 전에 주의해야할 점은 아래와 같은 경우를 고려하시면 되겠습니다.

``` plain
// 삭제 전
CCBDE
AAADE
AAABF
CCBBF

// 삭제 후
CCBDE
   DE
   BF
CCBBF
```

위의 경우처럼 2X3 형태로 A문자열이 연결되어 있을 경우 2X2 형태로 바로 삭제하면 안된다는 점입니다.

``` js
const test = [
  { m: 4, n: 5, board: ["CCBDE", "AAADE", "AAABF", "CCBBF"] },
  { m: 6, n: 6, board: ["TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ"] }
];

function friends4Block(m, n, board) {
  let arr = [];
  let removedBlockNum = 0;

  // 각 문자열 배열 만들기
  board.forEach((char) => arr = [...arr, char.split('')]);


  do {
    prevBlockNum = removedBlockNum;

    let arrIdx = [];  // 좌표 배열

    // 같은 블록 좌표 구하기
    for (let i = 0; i < m - 1; i++) {
      for (let j = 0; j < n - 1; j++) {
        if (arr[i][j] === arr[i][j+1] 
          && arr[i][j] === arr[i+1][j] 
          && arr[i][j] === arr[i+1][j+1]) {
          
          arrIdx = [...arrIdx, [i, j], [i, j+1], [i+1, j], [i+1, j+1]];
        }
      }
    }

    const filteredStrings = new Set(arrIdx.map(arr => JSON.stringify(arr)));  // string 변경 및 필터링
    const toArrIdx = [...filteredStrings].map(str => JSON.parse(str));  // 배열로 다시 변경

    toArrIdx.filter(x => arr[x[0]][x[1]] = ' ');  // 블록 삭제

    for (let k = 0; k < m; k++) {  // 위에 블록이 밑으로 이동 안했을 경우 대비해서 다시 한번씩 row만큼 스캔
      for (let i = 0; i < m; i++) {
        let temp;
        for (let j = 0; j < n; j++) {
          if (arr[i][j] === ' ') {
            if (!arr[i-1]) continue;  // 에러 예외 처리
            temp = arr[i-1][j];
            arr[i-1][j] = arr[i][j];
            arr[i][j] = temp;
          }
        }
      }
    }

    removedBlockNum = 0;

    arr.forEach(x => removedBlockNum += x.filter(y => y === ' ').length);

  } while (prevBlockNum != removedBlockNum); // 이전 삭제 블록 갯수랑 다르면 계속 반복

  return removedBlockNum;
}

for (let obj of test) {
  console.log(friends4Block(obj.m, obj.n, obj.board));
}

// output: 14, 15
```

* 문자 하나씩을 비교해야하기 때문에 <code>board</code> 배열에 담긴 문자열을 쪼개줍니다. 
* 열(<code>m</code>)과 행(<code>m</code>)만큼 이중 반복문으로 돌리면서 문자열을 체크합니다. 
* 2X2 형태의 문자열이 연결되어 있을 경우 배열 좌표를 <code>arrIdx</code> 배열에 담습니다. 좌표 배열을 담을때 중복되는 좌표값은 <code>new Set()</code>을 사용하여 걸러내고 다시 배열로 변경합니다.
* <code>filter</code> 함수를 각 좌표값의 블록을 삭제(공백 처리)해줍니다.
* 3중 loop 같은 경우 위에 남아 있는 블록을 아래로 이동시키기 위함입니다. (성능상 좋은 방식은 아닌것 같습니다. ㅠㅠ)
* <code>arr</code> 배열에서 공백처리된 부분의 갯수(length)값을 구하고 이전 블록 삭제 갯수랑 현재 블록 삭제된 갯수가 다르면 계속 처음부터 로직을 반복합니다.
* 반복을 다 마치면 현재 블록 삭제갯수를 리턴시킵니다.

![](../../../../images/algorithm/2018-kakao-blind-recruitment-round-1-6-07.png)

문제해설을 보니 이번 블라인드 채용 문제 중에 코드라인이 가장 길었던 것 같습니다. 아무래도 체크해야할 부분이 많아서 그랬던 것 같습니다. 생각해보니 지금까지 프로그래밍을 하면서 이렇게 많이 반복문을 많이 사용한 적은 처음인 것 같습니다.ㅋㅋ

### Wrap-up

결과적으로 문제는 해결했지만 포스팅에도 중간에 언급했듯이 성능상 안 좋은 코드들이 조금씩 보입니다. 다음 기회에 시간이 된다면 다른 방식으로 풀어서 성능을 개선시켜보겠습니다. 혹시 좀더 나은 알고리즘으로 푸신 분들은 댓글로 공유부탁드립니다. 

### Reference

[카카오톡 문제 및 해설](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)