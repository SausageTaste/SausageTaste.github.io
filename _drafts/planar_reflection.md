---
layout: post
title:  "평면 반사(Planar reflection)의 원리"
comments: true
use_math: true
---

지난번 포스트 ‘[게임이 반사를 구현하는 방법](/2020/11/16/2021-07-14-cg_reflections.html)’에서 다양한 반사 구현 기술에 대해 간단히 살펴보았었는데요.
평면 반사 파트의 경우 굉장히 단순하게 설명하고 넘어갔었지요.
거울이 바닥에 수평으로 놓여 있는 경우에 대해서만 설명했었습니다.
그때 이후로 좀 더 공부를 해서 마침내 그 어떤 각도나 위치에서도 완벽하게 작동하는 평면 반사를 만들어 왔습니다!

이번 글은 수학적인 분야에 더 깊게 들어갈 것입니다.
그러니 게임에서 사용되는 선형대수학에 대한 기초 지식이 있어야 내용 이해가 가능합니다.

# 공간 변환

반사상은 기본적으로 뒤집혀 있어야 합니다.
거울에 비친 얼굴은 좌우가 뒤집혀 있고, 수면에 미친 세상은 위아래가 뒤집혀 있지요.
지난 포스트에서 수면(水面) 반사를 구현할 때 이미지의 위아래를 뒤집는 방식으로 이를 구현했었는데요.
이를 수학적으로 표현하면 다음과 같습니다.

$$
\begin{bmatrix}
    1 &  0 & 0 \\
    0 & -1 & 0 \\
    0 &  0 & 1 \\
\end{bmatrix}
\times
\begin{bmatrix}
    x \\
    y \\
    x \\
\end{bmatrix}
=
\begin{bmatrix}
     x \\
    -y \\
     x \\
\end{bmatrix}
$$

단순하게 $y$축에 마이너스를 곱해서 위아래를 뒤집었었습니다.
$y$축이 상승 하강 방향을 표현하는 축이라고 했을 때, 이 방향을 따라 온세상을 위아래로 뒤집으면 그 결과는 물 반사와 같은 모습이 되겠지요.
즉, xz 평면을 기준으로 면대칭을 이루도록 뒤집어 주는 것입니다.

물론 이 방식대로 하면 큰 한계가 있습니다.
물이나 거울의 표면이 정확하게 xz 평면과 일치해야만 반사상의 위치가 정확합니다.

`임의의 평면으로 반사를 구현한 영상`

게리모드나 gta5에는 임의의 방향을 바라보는 거울이 등장합니다.
이런 것은 어떻게 구현한 것일까요?
아이디어는 다음과 같습니다.
주의깊게 읽어 주세요.

현재 행렬은 거울이 xz 평면과 일치할 때만 유효하다고 했지요?
그러니 거울을 포함한 온세상을 통채로 움직여서 거울이 xz와 일치하도록 해줍시다.
그러면 위에서 봤던 것처럼 y 값의 부호만 바꿔서 거울 반사상을 만들 수 있습니다.
그런 다음 온세상을 아까 통채로 움직이도록 했던 것의 반대로 움직여 줍니다.
그러면 마법처럼 딱 거울 위치를 기준으로 면대칭이 이루어진 반사상이 만들어 지게 됩니다.

참으로 복잡한 말이 아닐 수 없습니다.
복잡한 공간에 대한 문제는 그냥 수학적으로 생각하는 것이 차라리 더 편합니다.
지금부터 수학 기호를 통해 설명해 보겠습니다.

`거울와 장면 전체를 회전하는 그래픽`

거울이 xz 평면과 일치하도록 이동시키는 변환 행렬을 $A$로 두겠습니다.
이것이 바로 아까 말했던 온세상의 움직임입니다.
만약 행렬을 이용해 공간을 움직인다는 개념에 대해 잘 모르신다면 3Blue1Brown의 Linear Algebra 유튜브 동영상을 시청하고 와주시기 바랍니다.

이제 공간 전체를 거울을 기준으로 면대칭이 되도록 만드는 변환 행렬은 다음과 같이 구할 수 있습니다.

$$
M_{reflection} = A^{-1} \times
\begin{bmatrix}
    1 &  0 & 0 & 0 \\
    0 & -1 & 0 & 0 \\
    0 &  0 & 1 & 0 \\
    0 &  0 & 0 & 1 \\
\end{bmatrix}
\times A
$$

$A$는 공간의 수평이동도 수행해야 하므로 아핀 공간에서의 변환인 4x4 행렬이 되어야 합니다.
위의 식을 통해 얻은 행렬 $M_{reflection}$을 이용, 버택스 셰이더에서 다음과 같이 변환하면 반사상 이미지를 얻을 수 있습니다.

$$ \bold{v}^{\prime} = M_{projection} \times M_{view} \times M_{reflection} \times M_{model} \times \bold{v} $$

이 반사상 이미지를 거울이 그려지는 부분에 그대로 갖다 붙이면 완벽한 반사상이 완성됩니다.
여기까지는 그렇게 어렵지 않는 부분일 거라 생각합니다.
가장 어려운 부분은 행렬 $A$를 찾는 부분이겠지요?

$A$는 두 부분으로 이루어져 있습니다. 평행이동 행렬 $T$와 회전 행렬 $R$입니다.

$$ A = R \times T $$

$T$는 평면이 원점과 겹치도록 평행이동을 수행하는 행렬입니다.

`아무렇게나 놓여 있는 거울을 원점으로 옮기는 그림`

그리고 $R$은 거울 평면이 xz 평면과 수평이 되도록 원점을 중심으로 회전해 주는 행렬입니다.

`원점과 만난 거울이 수평이 되도록 회전시키는 그림`

행렬의 곱셈은 왼쪽에서 오른쪽으로 해나가지만, 변환의 적용 순서는 오른쪽에서 왼쪽입니다.
여기서 R을 통한 회전을 하려는 시점에는 T의 평행이동이 이미 적용되어 있는 상태입니다.
그러므로 원점과 딱 붙어 있는 평면을 원점을 기준으로 회전시키면 xz 평면과 일치하게 만들 수 있는 것이죠.

행렬 A의 특성을 시각적으로 한번 알아 봤는데요.
컴퓨터가 계산을 해내기 위해서는 이 모든 것이 숫자로 표현되어야겠죠?
지금부터 수학의 세계로 들어가 봅시다.

# 수학이 나설 차례

$T$와 $R$을 구하려면 우선 평면을 수학적으로 표현해야겠지요.
바로 고등학교 기하와 벡터 시간에 배운 **평면의 방정식**을 사용할 때입니다.

$$ ax + by + cz + d = 0 $$

여기서 $a$, $b$, $c$는 셋이 모여 3차원 벡터를 형성하면 그것은 해당 평면의 법선(normal) 벡터가 됩니다.
이것을 $n$으로 표기하겠습니다.
그리고 $n$을 길이가 1인 단위 벡터로 만든 것을 $\hat{n}$으로 포기하겠습니다.

이것을 사용하면 $T$와 $R$을 쉽게 구할 수 있습니다.

### 평행이동 $T$ 구하기

$T$를 구하기 위해서는 평면 위의 아무 점 하나를 선택해 줍시다.
평면의 방정식에서 x와 y 자리에 아무 숫자나 넣고 z에 대하여 정리하면 임의의 점 (x, y, z)를 구할 수 있지요. 그렇게 얻은 3차원 벡터를 $\bold{\alpha} = (\alpha_x, \alpha_y, \alpha_z)$로 두겠습니다.
그러면 T는 다음과 같습니다.

$$
T =
\begin{bmatrix}
    1 & 0 & 0 & -\alpha_x \\
    0 & 1 & 0 & -\alpha_y \\
    0 & 0 & 1 & -\alpha_z \\
    0 & 0 & 0 & 1 \\
\end{bmatrix}
$$

아핀 변환이 익숙하지 않은 분이라면 이게 뭔 괴상환 모양인가 하실지도 모르겠습니다.
그럴 때는 종이를 꺼내서 직접 계산을 해보면 한 방에 이해가 될 겁니다.

$$
T \times \bold{v} =
\begin{bmatrix}
    1 & 0 & 0 & -\alpha_x \\
    0 & 1 & 0 & -\alpha_y \\
    0 & 0 & 1 & -\alpha_z \\
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
    v_x -\alpha_x \\
    v_y -\alpha_y \\
    v_z -\alpha_z \\
    1 \\
\end{bmatrix}
$$

실질적으로는 $\bold{v} - \bold{\alpha}$를 4x4 행렬로 표현한 것에 불과하죠.
이렇게 하는 이유는 T를 R과 결합하기 위함입니다.

### 회전 $R$ 구하기

이제 R을 구할 차례입니다.
아까 말씀드린 대로, 회전을 적용하려고 하는 시점에서 거울에는 $T$ 변환이 적용되어 이미 원점과 접하고 있습니다.
이제 남은 것은 거울과 xz 평면이 같은 방향을 바라보고 있으면 되겠네요.

xz 평면이 바라보고 있는 방향은 3차원 벡터로

$$ \bold{n}_{xz} = (0, 1, 0) $$

입니다.
다들 이미 아시겠지요?
그럼 거울이 바라보고 있는 방향은 어떻게 알 수 있을까요?
답은 아까 봤던 평면의 방정식에 있습니다.

$$ ax + by + cz + d = 0 $$

여기서 평면이 바라보는 방향, 즉 법선 벡터는

$$ \bold{n} = (a, b, c) $$

처럼 구할 수 있지요.
계산의 편의성을 위해 둘 다 길이를 1로 정규화(normalize) 해줍시다.
$\bold{n}_{xz}$은 이미 길이가 1이므로 $\bold{n}$만 변환해 주면 되겠군요.

$$ \hat{\bold{n}} = \frac{\bold{n}}{||\bold{n}||} = \frac{(a, b, c)}{\sqrt{a^2 + b^2 + c^2}} $$

이 정도는 고등학교 기하와 벡터 시간에 배웠을 거라고 믿습니다!
그럼 이제 $n_{xz}$랑 $\hat{n}$이 일치하도록 만들어 주는 회전을 찾으면 되겠네요.

두 벡터 사이의 각도 $\theta$는 내적을 이용해 구할 수 있죠.

$$ \bold{n}_{xz} \cdot \hat{\bold{n}} = \cos \theta $$

$$ \theta = \arccos (\bold{n}_{xz} \cdot \hat{\bold{n}}) $$

그리고 회전축은 외적을 이용해 구할 수 있습니다.

$$ \bold{a} = (a_x, a_y, a_z) = \bold{n}_{xz} \times \hat{\bold{n}} $$

다 왔습니다!
이제 $axis$를 회전축으로 하여 $\theta$만큼 온세상을 회전시켜 주면 됩니다.
그런 행렬은 [이 글](https://www.euclideanspace.com/maths/geometry/rotations/conversions/angleToMatrix/index.htm)이 잘 설명하고 있으니 참고해 주세요.

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
    a_x a_x & a_x a_y & a_x a_z \\
    a_x a_y & a_y a_y & a_y a_z \\
    a_x a_z & a_y a_z & a_z a_z \\
\end{bmatrix}
+
\sin \theta
\begin{bmatrix}
    0    & -a_z & a_y  \\
    a_z  & 0    & -a_x \\
    -a_y & a_x  & 0    \\
\end{bmatrix}
$$

이것을 결합하여 4x4 행렬로 만들어 줍니다.

$$
R =
\begin{bmatrix}
    (1 - \cos \theta) a_x a_x + \cos \theta      &  (1 - \cos \theta) a_x a_y - a_z \sin \theta  &  (1 - \cos \theta) a_x a_z + a_y \sin \theta  &  0 \\
    (1 - \cos \theta) a_x a_y + a_z \sin \theta  &  (1 - \cos \theta) a_y a_y + \cos \theta      &  (1 - \cos \theta) a_y a_z - a_x \sin \theta  &  0 \\
    (1 - \cos \theta) a_x a_z - a_y \sin \theta  &  (1 - \cos \theta) a_y a_z + a_x \sin \theta  &  (1 - \cos \theta) a_z a_z + \cos \theta      &  0 \\
    0                  & 0                   & 0                   &  1 \\
\end{bmatrix}
$$

엄청나게 복잡하네요.
그래도 다행인 점은, 계산은 저희가 직접 할 필요는 없다는 점입니다.
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

이러한 과정을 거치면 마침내 거울을 xz 평면으로 이동시키는 행렬 $A = R \times T$를 찾을 수 있습니다.
가장 어려운 부분은 이제 끝났습니다!
이제 워에서 설명한 대로 행렬 $A$를 이용만 하면 완벽한 거울 대칭의 상을 만들어낼 수 있습니다.

# 다음은 포탈이다

바라건데 이 글을 읽은 분들이 평면 반사에 사용되는 수학을 제대로 이해하셨을 것입니다.
그렇다면 다음 단계로 넘어갈 준비가 되어 있습니다.

하프라이프를 개발한 Valve의 또 하나 유명한 게임, 포탈(Portal)에 등장하는 블루 포탈과 오렌지 포탈!
이것은 어떻게 구현되었을까요?
살짝 스포일러를 할까요?
거울과 마찬가지로, 포탈을 xz 평면으로 이동시키는 행렬을 찾음으로서 구현할 수 있습니다.

자세한 내용은 다음 글에서 뵙겠습니다.
