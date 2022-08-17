---
layout: post
title:  "던파M 레압 맞추는 기대 비용은 얼마일까?"
use_math: true
comments: true
---

# Ⅰ. 결론

결론부터 말씀드리겠습니다.

깔맞춤을 고려하지 않고, 바인드큐브를 1000 세라로 사고 아바타를 3만 테라로 사는 경우, 한 부위당 평균 **2만 3천 원**이 소모됩니다.
8개 부위를 깔맞춤 없이 다 맞춘다면 **18만 4천 원**이 들 거라고 기대할 수 있습니다.

상점 대신 경매장에서 바인드큐브를 조달한다면, 최저가 13,700테라 기준, 한 부위당 평균 **24만 8500테라**가 소모됩니다.
8개 부위를 깔맞춤 없이 다 맞춘다면 **198만 8천 테라**가 들 거라고 기대할 수 있습니다.

한 부위당 예상되는 소모 비용을 계산하는 공식은 다음과 같습니다.

$$ E(한 \ 부위당 \ 소모 \ 비용) = 6 \times (언커먼 \ 아바타 \ 가격) + 5 \times (바인드 \ 큐브 \ 가격) $$

이때 *언커먼 아바타 가격*과 *바인드 큐브 가격*은 원화(₩)면 원화, 테라면 테라 둘 중 하나로 통일해야 합니다.

# Ⅱ. 수학

우선 변수부터 설정하겠습니다.
언커먼 아바타의 가격을 $\alpha$로, 바인드큐브의 가격을 $\beta$로 둡니다.
이때 가격의 단위는 원화를 쓰든 테라를 쓰든 상관 없으나, $\alpha$와 $\beta$의 단위는 같아야 합니다.
여기서는 인게임 상점 기준을 적용해서 $\alpha = ₩3000$, $\beta = ₩1000$으로 두겠습니다.
아바타는 30,000테라이지만, 세라로 환산하고 원화로 환산하여 3,000으로 두었습니다.

$$ 1원 = 1세라 = 10테라 $$

아바타 합성을 했을 때 레압이 나올 확률은 $r$로 정합니다.
인게임 정보에 따르면 이 확률은 20%이므로 $r = 0.2$가 됩니다.
$r$은 확률값이기 때문에 $0 < r < 1$을 만족해야 합니다.
레압 당첨 확률이 0%나 100%가 되는 건 말이 안 되죠?

### 1. 비용 함수

이제 함수를 정의합니다.
n번째 시도에서 레압 뽑기에 성공했을 때 투여된 총 비용을 $T_n$으로 정의하겠습니다.
예를 들어 실패 없이 한 번에 레압 뽑기에 성공한 경우, 아바타 2개 값이랑 바인드큐브 하나 값이 들겠지요.
이를 식으로 나타내면 다음과 같습니다.

$$ T_1 = 2\alpha + \beta $$

두 번째에 성공했을 때 드는 총 비용은 다음과 같습니다.

$$ T_2 = \alpha + \beta + T_1 $$

처음에 실패하여 소모된 비용 $T_1$을 더해주고, 바인드큐브와 재료 아바타 하나씩을 더 사야 하므로 $\alpha + \beta$가 더해집니다.
던파M의 레압 뽑기 방식은, 언커먼 아바타 두 개를 합성해서 성공하면 레압이 튀어나오고 실패하면 언커먼 아바타가 튀어나옵니다.
실패하고 얻은 언커먼 아바타를 그대로 다시 재료로 쓰는 거죠.

$$
\begin{split}
T_3 &= \alpha + \beta + T_2 = 2(\alpha + \beta) + T_1 \\
T_4 &= \alpha + \beta + T_3 = 3(\alpha + \beta) + T_1 \\
T_5 &= \alpha + \beta + T_4 = 4(\alpha + \beta) + T_1 \\
    &... \\
T_n &= \alpha + \beta + T_{n - 1} = (n - 1)(\alpha + \beta) + T_1 \\
\end{split}
$$

패턴을 발견하셨나요?
이제 이것을 정리하면 완성입니다.

$$ T_n = (\alpha + \beta)n + \alpha $$

아까 위에서 $\alpha$와 $\beta$의 값을 결정했기 때문에

$$ T_n = (3000 + 1000)n + 3000 = 4000n + 3000 $$

이렇게 정리가 되기는 합니다만.
좀 더 일반화된 식을 찾기 위해서 계속 변수 $\alpha$와 $\beta$를 쓰겠습니다.
바인드큐브를 경매장에서 사는 분들은 변수 $\beta$의 값만 바꿔서 계산할 수 있도록 배려하는 차원에서 말이죠.

### 2. 확률 함수

이번에 정의할 함수는 $P_n$입니다.
n번째 합성에서 레압이 당첨될 확률을 말합니다.
$P_1$의 경우, 즉 한 번에 레압이 당첨될 확률은 당연히 20%겠죠?

$$ P_1 = r $$

2번째 합성에서 성공하려면, 첫 번째에서 실패하고 두 번째에 성공해야 합니다.

$$ P_2 = (1-r)r $$

이제 패턴을 찾아 봅시다.

$$
\begin{split}
P_3 &= (1-r)^2 r \\
P_4 &= (1-r)^3 r \\
P_5 &= (1-r)^4 r \\
    &... \\
P_n &= (1-r)^{n-1} r \\
\end{split}
$$

아까 $r$의 값을 0.2로 정했기 때문에 $P_n = 0.8^{n-1}0.2$로 쓸 수는 있지만, 이번에도 마찬가지로 일반화된 형태를 유지하겠습니다.

이것이 올바른 확률 함수가 맞나 확인해 볼까요?
한 번만에 뽑을 확률, 2번째에 뽑을 확률, 3번째에… 9999천 9999번째에 뽑을 확률… 등등 다 더했을 때 1이 나와야만 합니다.

왜냐하면 이게 우리가 상상할 수 있는 모든 경우의 수잖아요?
0번만에 뽑을 확률, -1번만에 뽑을 확률, 없죠.
1번부터 무한번까지 확률을 싹 다 더한다면 1이 나와야만 합니다.

$$
\begin{split}
1 & = \lim_{n \to \infty} (P_1 + P_2 + P_3 + ... + P_{9999 9999} + ... + P_n) \\
  & = \sum_{n=1}^{\infty} P_n \\
\end{split}
$$

따라서 위 등식이 성립해야만 합니다.

위키백과에서 찾을 수 있는 [이 공식](https://en.wikipedia.org/wiki/Summation?oldformat=true#Summation_index_in_exponents)을 이용하면 이 식을 쉽게 풀어헤칠 수 있습니다.

$$
\begin{split}
\sum_{n=0}^{k-1} x^n & = \frac{1-x^k}{1-x} \\
\sum_{n=1}^{k} x^{n-1}  & = \\
\end{split}
$$

이 공식에서 $k = \infty$, 그리고 $x = (1-r)$로 두면 풀 수 있겠군요.

$$
\begin{equation} \label{}
\begin{split}
\sum_{n=1}^{\infty} P_n & = \sum_{n=1}^{\infty} (1-r)^{n-1} r \\
                        & = r \sum_{n=1}^{\infty} (1-r)^{n-1} \\
                        & = r \frac{1-(1-r)^{\infty}}{1-(1-r)} \\
                        & = r \frac{1}{1-1+r} \\
                        & = \frac{r}{r} = 1 \\
\end{split}
\end{equation}
$$

$(1-r)^{\infty}$의 경우, $r$은 $0 < r < 1$을 만족하기 때문에 무한으로 곱하면 0으로 수렴합니다.
그래서 통째로 사라지는 것입니다.

이렇게 해서 우리가 만든 확률 함수 $P$가 올바르게 작동한다는 것을 증명했습니다.

Q.E.D.

아디오스.

![dnf_ranger_adios](/assets/images/misc_01/dnf_ranger_adios.jpg)

### 3. 결합

이제 레압 한 부위를 뽑기 위해 필요할 거라 예상되는 비용 $\lambda$를 계산할 차례입니다.
제가 하프라이프 시리즈의 팬이라서 람다를 거의 남용을 하고 있기 때문에 이해해 주시기 바랍니다.
아무튼 우리가 그토록 찾고자 하던 목표가 바로 이 $\lambda$입니다.
결론에서 이 값이 23,000원이라고 이미 알려드렸죠?
그 값이 어떻게 계산되었는가 알아봅시다.
이 여정의 끝이 머지 않았어요.

$$ \lambda = \lim_{n \to \infty} (P_1T_1 + P_2T_2 + P_3T_3 + P_4T_4 + ... + P_nT_n) $$

레압 한 부위를 뽑을 때, 당신은 한 번에 성공할 수도 있고, 두 번만에 성공할 수도 있고, 세 번만에 성공할 수도 있고, 백 번만에 성공할 수도 있고…….
이 모든 경우의 수에서 발생하는 비용, 즉 테라나 원화의 지출을 싹 다 더해주는데요.
그냥 더하는 것이 아니라, 각 경우가 발생할 수 있는 확률을 이용해 가중평균을 해줍니다.
그러면 레압 한 부위를 뽑을 때 드는 비용의 **기댓값(expected value)**을 구할 수 있습니다.

아까 $\sum_{n=1}^{\infty} P_n = 1$이 된다는 것을 확인했지요?
그렇기 때문에 무한 개의 항을 더해도 $\lambda$는 무한으로 발산하지 않고 어떤 값으로 수렴합니다.

$$ \lambda = \sum_{n = 1}^{\infty} P_nT_n = \sum_{n = 1}^{\infty} [(\alpha + \beta)n + \alpha](1-r)^{n-1}r $$

이걸 계산해야 합니다.
여기서 머리가 슬슬 아파오기 시작합니다만.
한번 해봅시다!

일단 식을 최대한 분리해 보겠습니다.

$$
\begin{split}
\lambda &= \sum_{n = 1}^{\infty} \left[ (\alpha + \beta)n(1-r)^{n-1}r + \alpha (1-r)^{n-1}r \right] \\
        &= \sum_{n = 1}^{\infty} (\alpha + \beta)n(1-r)^{n-1}r + \sum_{n = 1}^{\infty} \alpha(1-r)^{n-1}r \\
        &= (\alpha + \beta)\frac{r}{1-r} \sum_{n = 1}^{\infty} n(1-r)^n + \alpha r \sum_{n = 1}^{\infty} (1-r)^{n-1} \\
\end{split}
$$

여기서 두 번째 항은 아까 **식 (1)**에서 아디오스를 외쳤을 때 이미 계산을 끝내 놓았죠?

$$
\begin{split}
r \sum_{n=1}^{\infty} (1-r)^{n-1} & = \frac{r}{r} \\
  \sum_{n=1}^{\infty} (1-r)^{n-1} & = \frac{1}{r} \\
\end{split}
$$

이를 적용하면 다음과 같이 사라지게 됩니다.

$$
\begin{equation} \label{}
\begin{split}
\lambda & = (\alpha + \beta)\frac{r}{1-r} \sum_{n = 1}^{\infty} n(1-r)^n + \alpha r \left[ \sum_{n = 1}^{\infty} (1-r)^{n-1} \right] \\
        & = (\alpha + \beta)\frac{r}{1-r} \sum_{n = 1}^{\infty} n(1-r)^n + \alpha r \left[ \frac{1}{r} \right] \\
        & = (\alpha + \beta)\frac{r}{1-r} \sum_{n = 1}^{\infty} n(1-r)^n + \alpha \\
\end{split}
\end{equation}
$$

이제 첫번째 항만 풀면 되겠군요.
이번에는 아까 위키백과에 있는 공식 중 이것을 사용합니다.

$$
\begin{split}
\sum_{n=0}^{k-1}nx^n & = \frac{x - kx^k + (k - 1)x^{k+1}}{(1 - x)^2}
\end{split}
$$

$n(1-r)^n$의 형태는 딱 들어맞아서 다행이기는 한데, 시작 숫자와 끝 숫자가 달라서 그대로 적용하기는 힘들겠군요.
그러니 **식 (2)**의 첫번째 항 일부분을 좀 변형해준 다음 공식을 적용하도록 하겠습니다.

$$
\begin{equation} \label{}
\begin{split}
\sum_{n = 1}^{\infty} n(1-r)^n & = \sum_{n = 0}^{\infty - 1} n(1-r)^n &- \sum_{n = 0}^{0} n(1-r) &+ \sum_{n = \infty}^{\infty} n(1-r)^n \\
                               & = \lim_{k \to \infty} \frac{(1-r) - k(1-r)^k + (k - 1)(1-r)^{k+1}}{[1 - (1-r)]^2} &- 0(1-r)^0 &+ \lim_{k \to \infty} k(1-r)^k \\
\end{split}
\end{equation}
$$

$0(1-r)^0$ 이것이 0이 된다는 것은 명백해 보입니다만.
$\lim_{k \to \infty} k(1-r)^k$ 이 부분은 쉽지가 않네요.
[로피탈의 법칙](https://tutorial.math.lamar.edu/classes/calci/lhospitalsrule.aspx#:~:text=So%2C%20L'Hospital's%20Rule%20tells,and%20then%20take%20the%20limit.)을 사용할 때입니다.


$$
\begin{split}
\lim_{k \to \infty} k(1-r)^k &= \lim_{k \to \infty} \frac{k}{(1-r)^{-k}} \\
                             &= \lim_{k \to \infty} \frac{ \frac{d}{dk} k}{ \frac{d}{dk} (1-r)^{-k} } \\
                             &= \lim_{k \to \infty} \frac{1}{(1-r)^{-k} \ln (1-r) (-1)} \\
                             &= \frac{1}{\infty} \\
                             &= 0 \\
\end{split}
$$

0이 되어 사라지는군요!
$\lim_{k \to \infty} (k - 1)(1-r)^{k+1}$도 계산해 보면 마찬가지의 결과가 나옵니다.
이제 **식 (3)**를 완전히 풀 수 있습니다.

$$
\begin{split}
\sum_{n = 1}^{\infty} n(1-r)^n =& \lim_{k \to \infty} \frac{(1-r) - k(1-r)^k + (k - 1)(1-r)^{k+1}}{[1 - (1-r)]^2} &+ 0(1-r)^0 &+ \lim_{k \to \infty} k(1-r)^k \\
                               =& \frac{(1-r) - 0 + 0}{[1 - (1-r)]^2}                                             &+ 0        &+ 0 \\
                               =& \frac{1-r}{r^2} \\
\end{split}
$$

이걸 그대로 갖고 가서 **식 (2)**에 붙여넣으면 끝입니다.

$$
\begin{split}
\lambda &= (\alpha + \beta)\frac{r}{1-r} \left[ \sum_{n = 1}^{\infty} n(1-r)^n \right] + \alpha \\
        &= (\alpha + \beta)\frac{r}{1-r} \left[ \frac{1-r}{r^2} \right] + \alpha \\
        &= (\alpha + \beta)\frac{1}{r} + \alpha \\
        &= \frac{\alpha + \beta}{r} +\frac{r \alpha}{r} \\
        &= \frac{\alpha(r+1) + \beta}{r}
\end{split}
$$

처음 예상했던 것보다 엄청 단순한 형태로 정리되었네요.
여기서 $r=0.2$라는 점을 반영하면 $\lambda$ 식이 더더욱 간단해집니다.

$$ \lambda = 6 \alpha + 5 \beta \\ $$

### 4. 정리

엄청나게 멀고 험한 길을 헤쳐왔는데 마지막에 얻게 되는 결과물이 이리도 단순하다니.
이것이 바로 수학의 아름다움 아니겠습니까?

$$ \sum_{n = 1}^{\infty} [(\alpha + \beta)n + \alpha](1-0.2)^{n-1}0.2 = 6 \alpha + 5 \beta $$

좌변의 식을 최적화하고 압축해서 우변처럼 만드는 데 성공했습니다.
우리는 마침내 이 복잡했던 식을 오직 두 번의 곱셈과 한 번의 덧셈으로 압축하고, 그것이 올바른 결과를 도출할 수 있다는 것을 증명했습니다.
계산하기도 쉽고, 외우기도 쉽지요.

이제 이것을 이용하기만 하면 됩니다.
$\alpha$, $\beta$ 각각의 자리에 적절한 값을 넣고 계산하겠습니다.
아까 언커먼 아바타의 가격 $\alpha$를 3,000원으로, 바인드큐브 가격 $\beta$를 1,000원으로 뒀었죠?

$$
\begin{split}
\lambda_{상점} &= 6 \times 3000 + 5 \times 1000 \\
               &= 23000 \\
\end{split}
$$

이렇게 해서 23,000이라는 값이 나왔고 제일 위에서 말했던 결론이 나오게 된 것입니다.
한 부위당 레압이 뜨는 데 평균 23,000원이 소모된다는 사실을 알게 된 것입니다.

바인드큐브를 경매장에서 구입하는 경우도 알아볼까요?
지금 보니 바인드큐브 값이 최저가 13,700테라로 거래되고 있군요.
그러니 $\beta = 13700$입니다.
아까 $\alpha$와 $\beta$의 단위가 같아야 한다고 했죠?
언커먼 아바타의 가격은 30,000테라이므로 $\alpha = 30000$입니다.
그렇다면 결론은

$$
\begin{split}
\lambda_{경매장} &=  6 \times 30000 + 5 \times 13700 \\
                 &= 248500 \\
\end{split}
$$

24만 8500테라가 드는군요.

# Ⅲ. 후기

저는 과금을 하지 않고 레압을 맞추기로 계획을 세웠습니다.
그런데 하루에 벌 수 있는 인게임 돈의 양이 대체로 일정한 상황에서, 며칠이 지나야 레압을 다 맞출 수 있을지 계산해 보고 싶었습니다.
그래서 처음에는 그냥 C++를 사용해 백만 번 시뮬레이션 돌리는 방식으로 구할려고 했었는데요.

그런데 함수를 하나씩 정의하는 과정에서 문득 영감이 떠오르더군요.
함수에 시그마 summation이 많이 나오는데, 이것을 analytic 함수로 만들 방법이 존재한다면?
CPU를 혹사시키는 무식한 시뮬레이션을 할 필요가 없지 않을까?

OpenGL이나 Vulkan의 shader를 작성할 때도, BRDF나 atmospheric scattering을 구현할 때 최대한 numerical integration을 피할 수 있다면 유능한 개발자입니다.
저도 더 유능한 개발자가 되고 싶은 입장에서, 수학의 힘을 통해 최적화하는 경험을 한 번 해보고 싶었습니다.
그래서 고등학교 수학책도 펴보고 위키백과도 보면서, 맨땅에 헤딩 하듯 도전해 봤습니다.

결과적으로 굉장히 아름답고 단순명료한 수식을 얻게 되어 기쁩니다.

# Ⅳ. 참고자료

* [위키백과 Summation 문서](https://en.wikipedia.org/wiki/Summation?oldformat=true#Summation_index_in_exponents)
* [Paul's Online Notes](https://tutorial.math.lamar.edu/classes/calci/lhospitalsrule.aspx#:~:text=So%2C%20L'Hospital's%20Rule%20tells,and%20then%20take%20the%20limit.)
