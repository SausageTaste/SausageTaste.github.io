---
layout: post
title:  "컴퓨터에서 데이터의 표현"
comments: true
---


컴퓨터는 모든 데이터를 0과 1만으로 표현합니다.
이 0과 1만으로 다양한 도시, 인물, 마법효과 등등을 만들어낼 수 있습니다.
둘 사이의 간극이 너무 넓죠?
도대체 무슨 원리로 이것이 가능한 건지 감을 잡기 힘듭니다.
지금부터 천천히, 둘 사이의 간극을 메워 보도록 합시다!

## 3차원 물체는 삼각형의 집합

(대충 와이어프레임 그림)

우선 3차원 물체가 컴퓨터에서 어떻게 표현되는지 알아보도록 하겠습니다.
게임을 좋아하시는 분이라면 폴리곤이라는 말을 들어보셨을 텐데요.
그림처럼, 모든 물체는 수많은 삼각형으로 이루어져 있습니다.
아무리 복잡하게 생긴 물체라도 무수히 많은 삼각형으로 표현할 수 있죠.

(대충 다양한 물체의 와이어프레임)

>폴리곤의 뜻은 다각형입니다만, 컴퓨터그래픽스에서는 대부분 삼각형만을 사용합니다.  
사각형이나 오각형은 삼각형 여러 개를 합쳐서 만들죠.
삼각형만 사용하는 이유는, 삼각형만 사용할 때 더 렌더링을 빨리 할 수 있기 때문입니다.
블렌더에서는 사각형을 기본 단위로 사용합니다만….
자세한 내용은 다다음 글에서 설명드릴게요.

## 삼각형 하나는 9개의 숫자

삼각형을 0과 1만으로 표현하려면 우선 숫자로 표현할 수 있어야겠죠?
사실 이는 그리 어렵지 않습니다.
2차원 좌표평면에서 한 점을 표현하는 방법이 무엇이 있습니까?
숫자 두 개로 표현하는 것이죠?
아래 그림과 같이 점 하나당 숫자 2개를 사용해서 표현할 수 있습니다.

(대충 2차원 좌표평면에서 삼각형 그림)

3차원 좌표공간에서도 방식은 비슷합니다.
점 하나당 숫자를 3개씩 쓰면 되는 문제죠.

(대충 3차원 좌표공간에서 삼각형 그림)

위의 삼각형을 일렬로 나열해 보면 `(1, 2, 3), (4, 5, 6), (7, 8, 9)`처럼 적을 수 있습니다.
3차원 물체는 여러 개의 삼각형으로 이루어져 있다고 했죠?
그러니 3차원 물체를 숫자로 표현한다면 `(1, 2, 3), (4, 5, 6), (7, 8, 9), (1, 2, 3), (4, 5, 6), (7, 8, 9), ...`처럼 숫자를 죽 나열하기만 하면 됩니다!

아니, 이게 실화냐고요?

(대충 아무 모델 collada로 추출한 모습)

실제로 업계에서 사용되는 3차원 모델 파일을 열어보면 보시다시피 숫자가 잔뜩 나열된 것을 볼 수 있습니다!

이 숫자들을 0과 1만으로 표현하는 것은 어렵지 않죠.
위에서 보여줬던 삼각형 `(1, 2, 3), (4, 5, 6), (7, 8, 9)`을 0과 1로 표현하면 `010101011101010100010101010...`처럼 됩니다.