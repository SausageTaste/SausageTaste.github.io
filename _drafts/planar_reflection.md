---
layout: post
title:  "평면 반사(Planar reflection)의 원리"
comments: true
use_math: true
---

지난번 포스트 ‘[게임이 반사를 구현하는 방법](/2020/11/16/2021-07-14-cg_reflections.html)’에서 반사 효과를 구현하는 다양한 기술에 대해 간단히 살펴보았었는데요.
평면 반사의 경우 굉장히 단순하게 설명하고 넘어갔었지요.
거울이 바닥에 수평으로 놓여 있는 경우에 대해서만 설명했었습니다.
그때 이후로 좀 더 공부를 해서 마침내 그 어떤 각도나 위치에서도 완벽하게 작동하는 평면 반사를 만들어 왔습니다!

이번 글은 수학적인 분야에 더 깊게 들어갈 것입니다.
그러니 게임에서 사용되는 선형대수학에 대한 기초 지식이 있어야 내용 이해가 가능합니다.

# 거울 대칭을 만들어라

우선 좌표계부터 합의하고 진행하도록 할까요?
게이머들 사이에서는 “$y$축이 위 방향이다”, “$z$축이 위 방향이다”라고 주장하며 다투는 일이 종종 있습니다만.
실제로는 자신이 **좋아하는** 좌표계를 사용하면 되는 문제입니다.
저는 OpenGL에서 사용하는 오른손 좌표계를 좋아하니 때문에 그것을 사용하겠습니다.

![axis_planes](/assets/images/graphics_01/axis_planes.png)

여기서 빨간색 $xz$ 평면에 집중해 봅시다.
저번 글에서 설명한 평면 반사는 거울이나 수면(water surface, 水面)이 이 $xz$ 평면과 수평인 경우만 다뤘었습니다.
왜냐하면 이런 제한을 두면 굉장히 쉽게 평면 반사를 만들 수 있기 때문입니다.

반사상은 기본적으로 뒤집혀 있어야 합니다.
거울에 비친 얼굴은 좌우가 뒤집혀 있고, 수면에 미친 세상은 위아래가 뒤집혀 있지요.
$xz$ 평면을 기준으로 둔다면 세상을 위아래로 뒤집어야 합니다.

이를 수학적으로 표현하면 다음과 같습니다.

$$
\begin{bmatrix}
    1 &  0 & 0 \\
    0 & -1 & 0 \\
    0 &  0 & 1 \\
\end{bmatrix}

\begin{bmatrix}
    x \\
    y \\
    z \\
\end{bmatrix}
=
\begin{bmatrix}
     x \\
    -y \\
     z \\
\end{bmatrix}
$$

단순하게 $y$축에 마이너스를 곱해서 위아래를 뒤집었었습니다.
$y$축이 상승 하강 방향을 표현하는 축이라고 했을 때, 이 방향을 따라 온세상을 위아래로 뒤집으면 그 결과는 물 반사와 같은 모습이 되겠지요.
$xz$ 평면을 기준으로 면대칭을 이루도록 뒤집어 주는 것입니다.

여기서 벡터 $\boldsymbol{v} = [x, y, z]^T$는 공간 속에 있는 물체의 한 점을 의미합니다.
게임의 언어로 이야기하자면 각 폴리곤의 꼭짓점 위치를 뜻하고, 현실과 비교하자면 물체를 이루는 각 원자의 위치를 뜻합니다.
위 행렬을 이용해서 공간 속에 있는 모든 폴리곤의 꼭짓점을 상하 반전을 시켜주면 반사상 이미지가 완성되는 것이지요.

![planar_reflection_3_reflection](/assets/images/graphics_01/planar_reflection_3_reflection.jpg)

아까 말씀드렸듯이, 이 방식으로는 $xz$ 평면과 수평으로 놓인 거울이나 물의 반사만 구현할 수 있습니다.
참으로 다행스럽게도, 수평 조건만 만족한다면 높낮이 정도는 변화를 줄 수 있습니다.
만약 수면의 높이가 $h$라면 다음과 같은 방식으로 거울 반사상을 만들 수 있습니다.

먼저 $y$에 $h$를 빼서 수면의 높이가 0으로 오도록 만들어 줍니다.
그리고 원래 하듯이 -1을 곱해 주고요.
마지막으로 다시 $h$를 더해서 원래 높이로 돌려 놓으면 됩니다.

그러면 벡터는 다음과 같은 모습이 됩니다.

$$
\boldsymbol{v}' =

\begin{bmatrix}
     x \\
    -(y-h)+h \\
     z \\
     1 \\
\end{bmatrix}
=
\begin{bmatrix}
     x \\
    -y + 2h \\
     z \\
     1 \\
\end{bmatrix}
$$

이를 행렬로 표현하면 다음과 같습니다.

$$
\begin{bmatrix}
    1 & 0 & 0 & 0 \\
    0 & 1 & 0 & h \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1 \\
\end{bmatrix}

\begin{bmatrix}
    1 &  0 & 0 & 0 \\
    0 & -1 & 0 & 0 \\
    0 &  0 & 1 & 0 \\
    0 &  0 & 0 & 1 \\
\end{bmatrix}

\begin{bmatrix}
    1 & 0 & 0 &  0 \\
    0 & 1 & 0 & -h \\
    0 & 0 & 1 &  0 \\
    0 & 0 & 0 &  1 \\
\end{bmatrix}

\begin{bmatrix}
    x \\
    y \\
    z \\
    1 \\
\end{bmatrix}
=
\begin{bmatrix}
    1 &  0 & 0 &  0 \\
    0 & -1 & 0 & 2h \\
    0 &  0 & 1 &  0 \\
    0 &  0 & 0 &  1 \\
\end{bmatrix}

\begin{bmatrix}
    x \\
    y \\
    z \\
    1 \\
\end{bmatrix}
=
\begin{bmatrix}
     x \\
    -y + 2h \\
     z \\
     1 \\
\end{bmatrix}
$$

행렬 곱셈 연산을 수행하는 순서는 왼쪽에서 오른쪽이지만, 공간 변환을 수행하는 순서는 오른쪽에서 왼쪽입니다.
그러니 여기서는 $-h$를 먼저 더하고 곱하기 $-1$을 한 후 $+h$를 하는 것으로 이해해 주세요.

이것을 일반화시켜봅시다.
$y$에 -1을 곱하여 위아래로 뒤집는 행렬을 $F$로 (flip의 앞글자), $y$에 $h$를 빼서 거울이 $xz$ 평면과 겹쳐지도록 만드는 행렬을 $A$로 둡시다.

$$
F =
\begin{bmatrix}
    1 &  0 & 0 & 0 \\
    0 & -1 & 0 & 0 \\
    0 &  0 & 1 & 0 \\
    0 &  0 & 0 & 1 \\
\end{bmatrix}
, \
A =
\begin{bmatrix}
    1 & 0 & 0 &  0 \\
    0 & 1 & 0 & -h \\
    0 & 0 & 1 &  0 \\
    0 & 0 & 0 &  1 \\
\end{bmatrix}
$$

그러면 위의 복잡한 식은 다음과 같이 표기할 수 있습니다.

$$ M_{reflection} = A^{-1} F A $$

$$ \boldsymbol{v}' = M_{reflection} \boldsymbol{v} $$

여기까지 완성되었다면 여러분은 OpenGL이나 Direct3D의 GLSL/HLSL 셰이더 코드에서 거울 반사상을 렌더링하는 버텍스 셰이더를 만들 수 있습니다.

$$ \boldsymbol{v}_{clip \ space} = M_{projection} \times M_{view} \times M_{reflection} \times M_{model} \times \boldsymbol{v} $$

여기까지는 저번 글에서 설명한 수평 수면 반사 기술의 자세한 설명이었습니다.
하지만 우리의 여행은 여기서 끝나지 않습니다.
전신거울처럼 비스듬한 각도로 서있는 거울도 만들고 싶잖아요?

![mirror_in_gta5](https://steamuserimages-a.akamaihd.net/ugc/1817761489803604263/7522E83B7D17D392F4DDEC2D3B983448C503F817/?imw=5000&imh=5000&ima=fit&impolicy=Letterbox&imcolor=%23000000&letterbox=false)

<p><div style='position:relative; padding-bottom:calc(61.01% + 44px)'>
    <iframe
        src='https://gfycat.com/ifr/DishonestBlindJellyfish'
        frameborder='0'
        scrolling='no'
        width='100%'
        height='100%'
        style='position:absolute;top:0;left:0;'
        allowfullscreen
    ></iframe>
</div></p>

게리모드나 gtaV에는 임의의 방향을 바라보는 거울이 등장합니다.
이런 것은 어떻게 구현한 것일까요?
아이디어는 다음과 같습니다.

현재 행렬은 거울이 $xz$ 평면과 일치할 때만 유효하다고 했지요?
그러니 거울을 포함한 온세상을 통채로 움직여서 거울이 $xz$와 일치하도록 해줍시다.
아까는 상하로만 움직였는데, 이번에는 제한 없이 회전도 하고 이동도 시킵시다.
그리고 $y$ 값의 부호만 바꿔서 면대칭을 적용해 줍니다.
그런 다음 온세상을 아까 통채로 움직이도록 했던 것의 정확히 반대로 움직여 줍니다.
그러면 마법처럼 딱 거울 위치를 기준으로 면대칭이 이루어진 반사상이 만들어 지게 됩니다.

<p><div style='position:relative; padding-bottom:calc(56.25% + 44px)'>
    <iframe
        src='https://gfycat.com/ifr/FatherlyArtisticGoose'
        frameborder='0'
        scrolling='no'
        width='100%'
        height='100%'
        style='position:absolute;top:0;left:0;'
        allowfullscreen
    ></iframe>
</div></p>

희미한 흰색 정사각형이 바로 거울의 위치입니다.
거울이 놓여 있는 평면을 기준으로 온세상을 면대칭 시켜주면 위와 같이 보이게 됩니다.
이 동영상은 거울 대칭이 일어나는 과정을 보여드린 것이고요.
거울 대칭이 일어나기 전과 후 사진을 비교하면 다음과 같습니다.

![arbitrary_mirror_result_comparison](/assets/images/graphics_01/arbitrary_mirror_result_comparison.gif)

면대칭이 완료된 이후의 그림과 거울속 이미지가 정확하게 일치합니다.
거울을 시야에 두고 이리저리 움직여 보면, 상당히 정확해 보이는 거울이 완성되었다는 것을 확인할 수 있습니다.

프레임 드랍을 줄이기 위해 거울 반사상을 렌더링 할 때는 아무런 조명 효과도 적용하지 않아서 그런지 별로 예쁘지는 않네요.
gtaV의 거울 스크린샷도 보시면 그림자가 없는 걸 보니 비슷한 방식으로 최적화를 한 모양입니다.

<p><div style='position:relative; padding-bottom:calc(56.25% + 44px)'>
    <iframe
        src='https://gfycat.com/ifr/DeliriousHighGoldenretriever'
        frameborder='0'
        scrolling='no'
        width='100%'
        height='100%'
        style='position:absolute;top:0;left:0;'
        allowfullscreen
    ></iframe>
</div></p>

여기까지 잘 따라오셨다면 이거 하나는 이해하셨을 것입니다.

> 거울을 기준으로 면대칭 상(image, 像)을 만들어야 한다!

직관적으로 생각할 때는 쉽죠.
거울을 사이에 놓고 물건들의 위치를 양쪽으로 옮겨주면 되는 거 아닙니까?
하지만 컴퓨터로 이것을 계산하려고 하면 대략 머리가 멍해집니다.
계산을 하려면 위의 과정이 수학의 언어로 서술되어야 하니까요.
수학을 배워야지요.

준비 되셨나요?

# 가장 어려운 부분의 시작

아까 높낮이 조절이 되는 수평 평면 반사를 다시 떠올려 봅시다.

$$ M_{reflection} = A^{-1} F A $$

$A$를 어떻게 구했던가요?
거울이나 수면의 높이를 빼기만 했었죠?
이제 저희는 임의(arbitrary)의 움직임을 표현할 수 있어야만 합니다.

$A$는 **거울이 $xz$ 평면과 완전히 겹치도록 변환해주는 행렬**로 정의하겠습니다.
거울이 $xz$ 평면과 평행한 경우에는 쉽습니다.
그냥 높낮이만 조절해 주면 되니까요.
그런데 거울의 각도가 조금이라도 바뀐다면, 거울을 회전시키는 작업을 수행해야만 합니다.
평행이동도 하고 회전도 하는 등 무슨 수를 써서든 거울이 $xz$ 평면과 겹치기만 하면 장땡입니다.

$F$는 아까 설명했듯 그냥 $y$에 $-1$을 곱할 뿐입니다.
그러니 우리가 고생할 부분은 $A$를 찾아내는 작업 뿐입니다.

# $A$는 $R$과 $T$로 이루어져 있다

$A$는 두 부분으로 나누어 생각해 보겠습니다.
평행이동 행렬 $T$와 회전 행렬 $R$입니다.

$$ A = R \times T $$

$T$는 **T**ranslation의 앞글자로, 거울이 원점과 겹치도록 평행이동을 수행하는 행렬입니다.

![plane_to_origin_translate](/assets/images/graphics_01/plane_to_origin_translate.png)

그리고 $R$은 **R**otation의 앞글자로, 거울이 $xz$ 평면과 수평이 되도록 원점을 중심으로 회전해 주는 행렬입니다.

![plane_to_origin_rotate](/assets/images/graphics_01/plane_to_origin_rotate.png)

아까 말씀드렸듯이, 행렬의 변환은 오른쪽 먼저 적용됩니다.
여기서 $R$을 통한 회전을 하려는 시점에는 $T$의 평행이동이 이미 적용되어 있는 상태입니다.
그러므로 거울 평면은 이미 원점의 위치에 와 있는 상태입니다.
이때 거울 평면을 원점을 기준으로 회전시키면 $xz$ 평면과 일치하게 만들 수 있는 것이죠.

# 평면의 방정식

컴퓨터가 $T$와 $R$을 계한하려면 우선 평면을 수학적으로 표현해야 합니다.
바로 고등학교 기하와 벡터 시간에 배운 **평면의 방정식**을 사용할 때입니다.
아래처럼 생긴 거 기억 나시나요?

$$ ax + by + cz + d = 0 $$

여기서 a, b, c는 3차원 벡터를 이뤄서 평면의 법선(normal) 벡터가 되는데요.
이 법선 벡터의 길이는 1이어야 합니다.
즉, 다음 등식을 만족해야 합니다.

$$ \sqrt{a^2 + b^2 + c^2} = 1 $$

거울의 위치 정보를 이용해서 평면의 방정식을 만들 수 있지요.
평면은 세 개의 점을 이용해서 만들 수도 있고, 하나의 점과 하나의 방향을 가지고 만들 수도 있습니다.

거울의 모서리 세 개의 점 $\boldsymbol{p}$, $\boldsymbol{q}$, $\boldsymbol{r}$을 알고 있다고 가정합시다.
거울의 위치는 게임 맵 제작자가 원하는 대로 배치하는 것이므로, 거울의 꼭짓점 위치를 모를 리가 없지요.
이 정보를 이용하여 다음과 같이 평면의 법선 벡터를 구합니다.

$$ (a', b', c') = (\boldsymbol{q} - \boldsymbol{p}) \times (\boldsymbol{r} - \boldsymbol{p}) $$

이때, 법선 벡터는 정규화되어야 합니다.

$$ (a, b, c) = \frac{(a', b', c')}{\sqrt{a'^2 + b'^2 + c'^2}} $$

이제 내적을 이용해 마지막 남은 값 $d$를 찾으면 끝입니다.

$$ d = -(a, b, c) \cdot \boldsymbol{p} $$

이렇게 해서 거울과 겹치는 평면의 방정식 $ax + by + cz + d = 0$을 완성했습니다.
이것을 사용하면 $T$와 $R$을 쉽게 구할 수 있습니다.
컴퓨터 입장에서 쉽다는 것이지 사람에게는 어렵습니다. o_O

### 평행이동 $T$ 구하기

$T$를 구하기 위해서는 평면 위의 아무 점 하나를 선택해 줍시다.
평면의 방정식에서 $x$와 $y$ 자리에 아무 숫자나 넣고 $z$에 대하여 정리하면 임의의 점 $(x, y, z)$를 구할 수 있지요.

아무 점이라고 말하긴 했지만 말이죠.
사실 컴퓨터에는 부동소수점 정밀도 문제가 있기 때문에, 가능한 한 원점이랑 가까운 점을 선택하는 것이 좋습니다.
다음 공식을 사용하면 평면 위의 점들 중 원점과 가장 가까운 것을 알아낼 수 있습니다.

$$ \boldsymbol{\beta} = (\beta_x, \beta_y, \beta_z) =  -d(a, b, c) =  (-ad, -bd, -cd) $$

평면의 법선 벡터가 정규화되어 있어야 한다는 점 잊지 마세요.
이제 $T$는 다음과 같이 구할 수 있습니다.

$$
T =
\begin{bmatrix}
    1 & 0 & 0 & -\beta_x \\
    0 & 1 & 0 & -\beta_y \\
    0 & 0 & 1 & -\beta_z \\
    0 & 0 & 0 & 1 \\
\end{bmatrix}
$$

아핀 변환이 익숙하지 않은 분이라면 이게 뭔 괴상한 모양인가 하실지도 모르겠습니다.
그럴 때는 종이를 꺼내서 직접 계산을 해보면 한 방에 이해가 될 겁니다.

$$
T \times \boldsymbol{v} =
\begin{bmatrix}
    1 & 0 & 0 & -\beta_x \\
    0 & 1 & 0 & -\beta_y \\
    0 & 0 & 1 & -\beta_z \\
    0 & 0 & 0 & 1 \\
\end{bmatrix}
\times
\begin{bmatrix}
    v_x \\
    v_y \\
    v_z \\
    1 \\
\end{bmatrix}
=
\begin{bmatrix}
    v_x -\beta_x \\
    v_y -\beta_y \\
    v_z -\beta_z \\
    1 \\
\end{bmatrix}
$$

실질적으로는 $\boldsymbol{v} - \boldsymbol{\beta}$를 4x4 행렬로 표현한 것에 불과하죠.
아니 왜 굳이 이렇게 복잡하게 표기하냐고 물을 수 있을 텐데요.
그 이유는 $T$를 $R$과 결합하고, 그리고 projection, view, model 행렬과 합쳐서 단 하나의 행렬을 만들어 내기 위함입니다.
이 부분은 컴퓨터 그래픽스에 대한 지식이 있어야 이해가 가능할 것 같아요.

### 회전 $R$ 구하기

이제 $R$을 구할 차례입니다.
아까 말씀드린 대로, 회전을 적용하려고 하는 시점에서 거울에는 $T$ 변환이 적용되어 이미 원점과 접하고 있습니다.
이제 남은 것은 거울과 $xz$ 평면이 같은 방향을 바라보고 있으면 되겠네요.

$xz$ 평면이 바라보고 있는 방향은 3차원 벡터로 $\boldsymbol{n}_{xz} = (0, 1, 0)$입니다.
다들 이미 아시겠지요?
그럼 거울이 바라보고 있는 방향은 어떻게 알 수 있을까요?
답은 아까 봤던 평면의 방정식에 있습니다.

$$ ax + by + cz + d = 0 $$

여기서 거울 평면이 바라보는 방향, 즉 평면의 법선 벡터(normal vector)는 $\boldsymbol{n} = (a, b, c)$입니다.
계산의 편의성을 위해 둘 다 길이를 1로 정규화(normalize) 해줍시다.
$\boldsymbol{n}_{xz}$은 이미 길이가 1이므로 $\boldsymbol{n}$만 변환해 주면 되겠군요.

$$ \hat{\boldsymbol{n}} = \frac{\boldsymbol{n}}{||\boldsymbol{n}||} = (\frac{a}{\sqrt{a^2 + b^2 + c^2}}, \frac{b}{\sqrt{a^2 + b^2 + c^2}}, \frac{c}{\sqrt{a^2 + b^2 + c^2}}) $$

이 정도는 고등학교 기하와 벡터 시간에 배웠을 거라고 믿습니다!
그럼 이제 $n_{xz}$랑 $\hat{n}$이 일치하도록 만들어 주는 회전을 찾으면 되겠네요.

두 벡터 사이의 각도 $\theta$는 내적을 이용해 구할 수 있죠.

$$ \boldsymbol{n}_{xz} \cdot \hat{\boldsymbol{n}} = \cos \theta $$

$$ \theta = \arccos (\boldsymbol{n}_{xz} \cdot \hat{\boldsymbol{n}}) $$

그리고 회전축은 외적을 이용해 구할 수 있습니다.

$$ \boldsymbol{\alpha} = (\alpha_x, \alpha_y, \alpha_z) = \boldsymbol{n}_{xz} \times \hat{\boldsymbol{n}} $$

다 왔습니다!
이제 $\boldsymbol{\alpha}$를 회전축으로 하여 $\theta$만큼 온세상을 회전시켜 주면 됩니다.

$$
R =
\cos \theta
\begin{bmatrix}
    1 & 0 & 0 \\
    0 & 1 & 0 \\
    0 & 0 & 1 \\
\end{bmatrix}
+
(1 - \cos \theta)
\begin{bmatrix}
    \alpha_x \alpha_x & \alpha_x \alpha_y & \alpha_x \alpha_z \\
    \alpha_x \alpha_y & \alpha_y \alpha_y & \alpha_y \alpha_z \\
    \alpha_x \alpha_z & \alpha_y \alpha_z & \alpha_z \alpha_z \\
\end{bmatrix}
+
\sin \theta
\begin{bmatrix}
    0    & -\alpha_z & \alpha_y  \\
    \alpha_z  & 0    & -\alpha_x \\
    -\alpha_y & \alpha_x  & 0    \\
\end{bmatrix}
$$

어떻게 이렇게 괴상하게 생긴 행렬이 만들어졌나 궁금하신 분들은 [이 글](https://www.euclideanspace.com/maths/geometry/rotations/conversions/angleToMatrix/index.htm)을 참고해 주세요.

이게 이것을 결합하여 4x4 행렬로 만들어 줍니다.

$$
R =
\begin{bmatrix}
    (1 - \cos \theta) \alpha_x \alpha_x + \cos \theta &
    (1 - \cos \theta) \alpha_x \alpha_y - \alpha_z \sin \theta &
    (1 - \cos \theta) \alpha_x \alpha_z + \alpha_y \sin \theta &
    0 \\

    (1 - \cos \theta) \alpha_x \alpha_y + \alpha_z \sin \theta &
    (1 - \cos \theta) \alpha_y \alpha_y + \cos \theta &
    (1 - \cos \theta) \alpha_y \alpha_z - \alpha_x \sin \theta &
    0 \\

    (1 - \cos \theta) \alpha_x \alpha_z - \alpha_y \sin \theta &
    (1 - \cos \theta) \alpha_y \alpha_z + \alpha_x \sin \theta  &
    (1 - \cos \theta) \alpha_z \alpha_z + \cos \theta      &
    0 \\

    0 & 0 & 0 & 1 \\
\end{bmatrix}
$$

엄청나게 복잡하네요.
그래도 다행이도, 계산을 저희가 직접 할 필요는 없습니다.
컴퓨터에게 시키면 되니까요.

C++ 코드로는 다음과 같습니다.

```cpp
glm::mat4 roate_about_axis_1(const float radians, const glm::vec3& axis) {
    glm::mat4 output{1};

    const glm::vec3 axis_n = glm::normalize(axis);

    const float c = cos(radians);
    const float s = sin(radians);
    const float t = 1.f - c;
    const float x = axis_n.x;
    const float y = axis_n.y;
    const float z = axis_n.z;

    output[0][0] = t*x*x + c;
    output[1][0] = t*x*y - z*s;
    output[2][0] = t*x*z + y*s;

    output[0][1] = t*x*y + z*s;
    output[1][1] = t*y*y + c;
    output[2][1] = t*y*z - x*s;

    output[0][2] = t*x*z - y*s;
    output[1][2] = t*y*z + x*s;
    output[2][2] = t*z*z + c;

    return output;
}

glm::mat4 roate_about_axis_2(const float radians, const glm::vec3& axis) {
    const glm::quat q = glm::angleAxis(radians, glm::normalize(axis));
    return glm::mat4_cast(q);
}
```

두 함수 모두 완전히 같은 일은 하기 때문에 둘 다 올바르게 작동합니다.
`roate_about_axis_2`가 좀 더 유지보수 하기 좋아 보이죠?

이제 $R$과 $T$ 모두 구했습니다!
이제 둘을 곱하기만 하면 A를 구할 수 있습니다.
그런데 이 복잡한 행렬을 어떻게 곱할지 아찔하네요.
뭐, 컴퓨터가 알아서 하겠죠?
그걸 왜 사람이 합니까?

```cpp
const glm::mat4 T = ...
const glm::mat4 R = roate_about_axis(...);
const glm::mat4 A = R * T;
```

가장 어려운 부분은 이제 끝났습니다!
이제 위에서 설명한 대로 반사 행렬을 $M_{reflection} = A^{-1}FA$처럼 만들면 완벽한 거울 대칭의 이미지를 만들 수 있습니다.

# 다음은… Portal

바라건데 이 글을 읽은 분들이 평면 반사에 사용되는 수학을 제대로 이해하셨을 것입니다.
그렇다면 다음 단계로 넘어갈 준비가 됐습니다.

하프라이프를 개발한 Valve의 또 하나 유명한 게임, 포탈(Portal)에 등장하는 블루 포탈과 오렌지 포탈!
이것은 어떻게 구현되었을까요?

살짝 스포일러를 할까요?
거울과 마찬가지로, 포탈을 $xz$ 평면으로 이동시키는 행렬을 찾음으로서 구현할 수 있습니다.
대신 거울은 원점으로 옮겼다 다시 원래 자리로 돌아가는 반면, 포탈은 반대쪽 포탈이 있는 장소로 이동해야 한다는 차이가 있지요.
그리고 거울은 원점으로 이동한 다음 $-y$를 해서 면대칭을 만들어주는 반면, 포탈은 180도 회전을 해줘야 합니다.
그 외에도 $T$와 $R$을 구할 때 유의해야 할 점이 추가로 있군요.

이 정도 힌트를 갖고 스스로 생각해 보는 건 어떨까요?
나름 괜찮은 연습이 될 것 같네요.
그럼, 다음 글에서 뵙겠습니다!
