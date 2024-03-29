---
layout: post
title: “포탈 게임이 구현된 원리”
comments: true
use_math: true
---

제가 포탈이라는 게임을 처음 접한 것은 중학교 1학년 때였습니다.
카운터스트라이크 소스를 보유하고 있던 스팀 계정이 해킹당하여 크게 상심하고 있었던 당시, 제 눈길을 끈 새로운 게임은 바로 팀포트리스 2였습니다.
그때는 게임을 디지털로 구매한다는 개념이 익숙하지 않던 때라 패키지를 구입할 생각을 했었습니다.
그렇게 부모님께 졸라서 구매한 오렌지 박스.
드디어 꿈에 그리던 팀포2를 할 수 있게 되어 너무나 기쁜 순간이었습니다.

![orange_box_and_css](/assets/images/graphics_01/orange_box_and_css.jpg)

그런데 오렌지 박스라는 패키지 안에는 팀포트리스 2 외에도 우주명작 두 개가 더 포함되어 있었죠.
하프라이프 2와 포탈.
둘 다 팀포2에 뒤지지 않는 훌륭한 게임입니다.
온라인 멀티플레이어 게임만 하던 제게 있어 그 두 게임은 완전히 새로운 세상이었습니다.

[스팀에 업로드한 스크린샷](https://steamcommunity.com/sharedfiles/filedetails/?id=35076208)을 보면, 포탈 2를 예약구매까지 해서 플레이 했을 정도로 엄청난 팬이었음을 알 수 있습니다.

누구나 처음 포탈이라는 게임을 접한다면 그 신비로운 비주얼에 매료되지 않을 수 없을 것입니다.
텔레포트라 하면 화면이 암전되며 순식간에 다른 지점으로 이동해버리는 일반적인 것과는 다르게, 포탈에서는 공간의 경계를 그저 걸어서 넘나들죠.

중학생 시절부터 이것의 원리에 대해서 쭉 궁금했었지만, 그것을 이해할 수 있을 정도로 똑똑해지기까지 거의 10년이 걸린 것 같군요.

저는 앞서 평면 반사의 원리 글에서 포탈의 원리에 대한 포스트를 예고했었습니다.
그 이유는 포탈의 원리를 이해하기 위해 평면 반사의 원리를 이해할 필요가 있기 때문입니다.
일반적으로는 카메라의 위치를 두 포탈쌍의 상대적 위치에 따라 변환하는 방식을 사용하는 것이 더 직관적입니다만.
저는 평면 반사를 구현할 때 평면을 이용해 반사상 행렬을 계산해낸 것처럼, 두 포탈 평면을 이용해 포탈 행렬을 구할 것입니다.
그러므로 해당 글을 꼭 먼저 읽어주세요.

# 온세상을 움직여라

평면 반사의 원리를 이해하셨다면 바로 감이 오셨을 거라고 생각합니다.
그때는 온세상을 거울 기준 면대칭을 한 이미지를 반사상으로 사용했었죠?
이번에도 비슷합니다.
당신이 바라보고 있는 포탈을 1번, 건너편 포탈을 2번이라고 두겠습니다.
포탈 1에 그려져야 할 이미지를 얻기 위해서는, 포탈 2의 앞에 있는 모든 물체들을 끌어와서 포탈 1 뒤에다 딱 붙여야 합니다.
