---
title: "Introduction to Reinforcement Learning"
tags: [deeplearning, reinforcementlearning]
---

강화학습 처음 시작하기

<!--more-->

한 해 동안 Reinforcement learning 관련 연구에 참여하게 됐다. 간략하게 말하면 스타크래프트2 전투에 사용되는 AI 모델을 만드려고 한다. DeepMind를 이겨보자! 라는 다소 야심만만한 포부를 갖고 모였다. 다만 나는 강화학습에 첫 입문한 새내기라 한 학기 동안에는 이론 공부에 집중하기로 했다. 

새로운 공부를 시작하는 건 조금 막막하지만 설레는 일이기도 하다.

첫 입문용 책으로는 *파이썬과 케라스로 배우는 강화학습*을 추천받았다. 최소한의 수식과 직관적인 그림을 통해 강화학습을 이해한다, 는 이 책의 깔끔한 목표가 마음에 들었다.

이 다음으로는 [Silver 교수님의 강의](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html)를 들어보려고 한다.

다음 단계로 넘어가기 이전에, 이 책을 바탕으로 좀더 개론적이고 직관에 의존한 설명으로 스스로 강화학습의 개념을 정리해 보고자 한다.

## 강화학습, 행동심리학, 머신러닝

'강화'라는 말은 행동심리학에서 처음 정의된 개념이다. 강화란 동물이 시행착오(Trial and Error)를 통해 학습하는 방법 중 하나를 일컫는다. 행동과 그 행동의 결과로 나타나는 '보상'을 연결함으로써 보상을 얻게 해 주는 행동을 더 많이 하게 된다. 바로 이것이 강화학습의 모티프다.

머신러닝을 크게 지도학습, 비지도학습, 강화학습으로 나누곤 한다. 즉, 강화학습은 지도학습도, 비지도학습도 아니다. 정답이 주어진 것(Supervised)도 아니고, 주어진 데이터로만 학습하는 것(Unsupervised)도 아니기 때문이다. 간단히 말해, 강화학습은 보상(Reward)를 통해서 학습한다. 이는 직접적인 정답이 아니라 간접적으로 정답을 이끌어내는 지침이 된다.

## 강화학습은 환경에 대한 사전지식이 없어도 학습한다

- 다양한 환경에 대한 정보를 미리 알기 힘든 경우가 있다. (시뮬레이션)

- 미리 아는 환경이라도 환경에 대한 정보를 통해 계산하려면 많은 시간이 걸리는 경우가 있다. (바둑: 아주 많은 가능한 상태 > $$ 10^{360} $$)

## MDP (Markov Decision Process)

강화학습은 **순차적으로 결정을 내려야 하는 문제**에 적용된다. 순차적으로 결정을 내리는 과정을 MDP로 정의하는데, MDP를 구성하는 요소는 다음과 같다.

- State, or Observation
    - 에이전트의 관찰 가능한 상태
    - 에이전트는 자신의 관찰을 통해 상황을 판단해서 행동을 결정
- Action
    - 에이전트가 어떠한 상태에서 취할 수 있는 행동
- Reward
    - 보상을 통해 자신이 했던 행동들을 평가하고, 어떤 행동이 '좋은 행동'인지 알 수 있다.

- State Transition Probability
- Discount Factor
- Policy
    - MDP에서 구해야 할 답
    - 모든 상태에 대해 어떤 행동을 해야 할지 정해놓은 것 ($$ action = pi(state) $$)
    - 보상의 합을 최대로 하는 정책: optimal policy

강화학습에서는 에이전트의 상태와 그에 따른 보상을 통해 다음 Action을 결정한다.

고전적인 MDP의 해결법은 다음과 같다.

- Dynamic Programming
- Evolutionary Algorithm (e.g. Genetic Programming)

이들이 가진 한계점을 강화학습을 통해 보완할 수 있다. 

## 가치함수, 큐함수

강화학습은 MDP로부터 최적의 정책을 찾는 과정이다. 최적의 정책을 구하기 위해, '현재 상태로부터 앞으로 얼마의 보상을 받을 수 있을 것인지에 대한 기댓값'의 개념을 생각해 볼 수 있다. 현재의 상태를 입력값으로 받아 앞으로 받을 보상의 합을 결과로 내놓는 함수를 가치함수라고 한다.

**가치함수는 정책을 고려해야 한다.** 정책에 따라서 각 상태에서 선택하는 액션이 달라지기 때문에, 계산하는 가치함수의 값 또한 달라지기 때문이다. 강화학습에서 정책을 고려한 가치함수의 식은 다음과 같다.

$$  v_\pi(s) = E_\pi[R_{t+1} + \gamma v_\pi(S_{t+1}) | S_t = s] $$

### 큐함수

에이전트는 가치함수를 구함으로써 이 상태가 얼마나 '좋은지' 판단할 수 있다. 즉 에이전트는 더 좋은 상태로 가는 것, 더 큰 값의 가치를 갖는 상태로 이동하는 액션을 취할 것이다. '행동'을 통해 가치함수를 구하는 과정이 강화학습에서 빈번하게 일어나기 때문에, 현재 상태에서 어떤 액션을 취했을 때 이동하는 상태의 가치함수를 따로 정의할 수 있다. (행동 가치함수)

이를 큐함수라고도 부른다. 큐함수는 현재 상태와 행동(action)을 함께 입력으로 받는다.

$$ q_\pi(s, a) = E_\pi[R_{t+1} + \gamma q_\pi(S_{t+1}, A+{t+1}) | S_t = s, A_t = a]$$

### 벨만 방정식

벨만 방정식은 현재 상태의 가치함수와 다음 상태 가치함수의 관계식이다. 벨만 기대 방정식은 특정 정책 $$ \pi $$에 대한 참 가치함수를 구하는 과정이고, 벨만 최적 방정식은 이 값을 최대로 하는 최적 정책에 대한 가치함수의 관계식이다.

$$ v^*(s) = max_aE[R_{t+1} + \gamma v^*(S_{t+1}) | S_t = s, A_t = a] $$

## 다이나믹 프로그래밍에서 강화학습으로

고전적으로 MDP를 푸는 방법 중, 대표적으로 다이나믹 프로그래밍이 있다. 정확히는 벨만 방정식을 푸는 방법으로 제시된 것이다. 다이나믹 프로그램의 핵심은 큰 문제를 바로 푸는 것이 아니라 작은 문제들을 시간에 따라 다른 프로세스로 풀겠다는 것이다. 이 과정에서 각 문제들의 해답을 재사용할 수 있고, 계산량을 일정 부분 줄일 수 있다. (memoization)


## 정책 이터레이션

벨만 기대 방정식을 이용한다. 처음부터 최적 정책을 찾을 수는 없으므로, 무작위의 정책에서 시작해서 더 나은 정책으로 발전시키는 과정이다. 크게 정책 평가(Policy Evaluation)와 정책 발전(Policy Improvement)의 단계로 나누어진다.

### 정책 평가

가치함수를 통해 정책의 우열을 판단할 수 있다. 가치함수의 정의를 통해 현재 정책에 대한 가치함수의 실제 값을 구할 수 있다. 이 과정에서 이전의 결과를 이용하여 계산량을 줄이는 것이 다이나믹 프로그래밍이다. 한 번의 정책 평가 과정에서, '현재 시점에' 다음 상태에 저장된 가치함수를 이용해 가치함수의 근삿값을 구한다. 이를 **모든 상태에 대해 동시에** 여러 번 반복하면 참 가치함수로 수렴한다.

### 정책 발전

정책 평가를 발전으로 다양한 방법을 통해 정책을 발전시킬 수 있다. 가장 간단한 형태가 Greedy Policy Improvement이다. 이는 이전 정책으로 나온 가치함수에서, 가치함수의 값이 가장 커지는 상태로 이동하는 액션을 통해 새로운 정책을 형성하는 것이다. 탐욕 정책 발전으로 얻은 새로운 정책은 $$ \pi'(s) = argmax_{a \in A} q_\pi(s, a) $$가 된다.

## 가치 이터레이션

벨만 최적 방정식을 이용한다. 정책 이터레이션에서는 가치함수(정책 평가의 도구)와 정책을 명시적으로 분리해놓은 데에 반해, 가치 이터레이션에서는 가치함수 내에 정책의 개념을 포함하여, 가치함수의 업데이트와 정책 발전이 함께 이루어지도록 한다.

벨만 최적 방정식을 풀어서 최적 정책을 바로 구할 수 있다. 즉 어떤 액션을 통해 이동 가능한 다음 상태 중 가치함수의 값이 가장 높은 상태만 취하겠다는 것이다. 

$$ v_{k+1}(s) = max_{a \in A}(R_s^a + \gamma v_k(s')) $$

## DP의 한계와 강화학습

이론적으로는 DP를 사용하여 벨만 방정식을 풀고, 그 결과 최적 정책을 얻어낼 수 있지만 실제 상황에 적용할 수 없는 현실적인 문제들이 존재한다.

1. 계산 복잡도와 차원의 저주(Curse of Dimensionality): 환경이 포함하는 전체 상태의 갯수 n에 대해 DP의 계산복잡도는 $$ O(n^3) $$ 이다. 또한 상태의 차원이 늘어남에 따라 상태의 수가 지수적으로 늘어나기 때문에 2차원 이상의 문제에 대해서는 현실적으로 계산 불가능한 시간 복잡도를 가진다.

2. 환경에 대한 완벽한 정보가 필요: 보상과 상태 변환 확률을 이미 정확히 알고 있다는 가정 하에서 방정식을 풀 수 있다.

강화학습은 환경과의 상호작용을 통해, 환경의 정보를 완벽하게 알지 못하더라도 '경험'을 바탕으로 학습할 수 있다. 즉, 강화학습에서는 환경의 모델 ($$ P_{ss'}^a, R_s^a $$)이 필요없다.

거의 모든 강화학습의 방법들은 GPI(Generalized Policy Iteration)의 관점에서 볼 수 있다. GPI는 policy 와 value function의 추정치를 갖고 상호작용하는 과정이다. 정책 평가(Policy Evaluation) 과정에서 참 가치함수로 수렴할 때까지 계산하지 않고 단 한번만 정책 평가를 통해 가치함수를 업데이트하고, 바로 정책을 발전시키는 과정을 반복해도 결과적으로는 optimal value function과 optimal policy를 찾아낼 수 있다는 것이다.

## 강화학습: 예측 (Prediction)

환경과의 상호작용을 통해 주어진 정책에 대한 가치함수를 학습하는 것. 

### 몬테카를로 예측

에이전트가 한 번 환경에서 에피소드를 진행(샘플링) -> 몬테카를로 근사를 사용하여 가치함수 추정

### 시간차 예측

한 에피소드가 끝날 때까지 기다리는 것이 아니라 타임스텝마다 가치함수를 업데이트

업데이트의 목표가 정확하지 않은 상태에서, 다음 상태의 가치함수와 알게 된 보상을 더해 그 값을 업데이트의 목표로 한다.

시간차 예측은 다음 state의 추정치(estimation)을 기반으로 현재 state의 추정치를 업데이트한다. 이러한 아이디어를 **bootstrapping**이라고 한다. 많은 reinforcement learning에서 효율성을 위해 bootstrapping을 사용한다.

$$ V(S_t) \leftarrow V(S_t) + \alpha (R + \gamma V(S_{t+1} - V(S_t))) $$

## 강화학습: 제어 (Control)

예측 + 정책 발전을 함께 수행

### $$ \epsilon $$-greedy policy improvement

탐욕 정책에서는 에이전트가 현재 상태에서 가장 큰 가치를 지니는 행동을 선택한다. 그러나 초기의 에이전트에게 탐욕 정책은 잘못된 학습으로 가게 할 가능성이 크다. $$ \epsilon $$-greedy 정책을 통해 특정 확률로 탐욕적이지 않은 행동을 선택하도록 한다.

![](https://www.dropbox.com/s/1lg79ubpxpk0iwz/e-greedy.png?raw=1)


### 시간차 제어 (SARSA)

시간차 예측 + $$ \epsilon $$-greedy 정책

1. $$ \epsilon $$-greedy 정책을 통해 샘플 $$ [S_t, A_t, R_{t+1}, S_{t+1}, A_{t+1}] $$ 을 획득

2. 획득한 샘플로 큐함수 $$ Q(S_t, A_t) $$ 를 업데이트

![](https://www.dropbox.com/s/1f9epyjnppnvbkc/sarsa.png?raw=1)


살사는 on-policy 시간차 제어이다. 즉, 자신이 행동하는 대로 학습한다. 이때, 탐험에 의해 Optimal Path에 있는 State의 Q함수가 비정상적으로 낮은 값을 갖게 되고, 최적 정책을 학습하지 못하는 경우가 생긴다. (일종의 갇혀버리는 현상)

이를 해결하기 위해 off-policy로 학습하는 제어의 필요성이 생겼고, off-policy temporal difference control = Q-learning이 제안되었다.

### 오프폴리시 시간차 제어 (Q-learning)

행동하는 정책과 학습하는 정책을 따로 분리해서 생각

행동하는 정책은 $$ \epsilon $$-greedy 정책, 학습하는 정책은 벨만 최적 방정식에 따라 다음 상태에서 가장 큰 큐함수를 가지고 업데이트.

그러므로 탐험을 하더라도 학습된 큐함수가 계속 optimal한 값을 향해 발전한다.

![](https://www.dropbox.com/s/sken8x5k7hw6fl3/q-learning.png?raw=1)


## 인공신경망을 통한 큐함수 근사


### Deep SARSA (Value-based)

지금까지 살펴본 SARSA, Q-learning의 경우에는 아직 인공신경망을 활용하지 않고, 테이블을 기반으로 간단히 표현했다. 하지만 가능한 상태의 갯수가 수없이 많은 현실의 문제에서는 이러한 알고리즘들이 별 도움이 되지 않는다. 그러므로 큐함수를 테이블의 형태로 모든 행동 상태에 대해 업데이트하고 저장하는 대신, **매개변수로 근사하여** 사용한다. 이때 근사함수를 구하기 위해 **인공신경망**이 사용된다. 

즉, 큐함수 업데이트가 테이블을 통해 직접 계산해서 이뤄지는 것이 아니라, 상태를 input으로 받고 큐함수 자체를 output으로 반환하는 인공신경망 모델을 정의하고 지속적으로 학습시킨다. Loss Function을 정의하는 것이 중요하다. 살사에 딥러닝을 적용한 Deep SARSA의 경우 오차함수는 MSE를 이용해 다음과 같이 정의한다.

$$ MSE = (target - prediction)^2 = (R_{t+1} + \gamma Q(S_{t+1}, A_{t+1}) - Q(S_t, A_t))^2 $$

## REINFORCE (Policy-based)

인공신경망이 직접 정책을 근사. 즉, 정책을 정책신경망으로 근사한다. 목표함수를 정의하고 이를 최대화하는 방향으로 Gradient Ascent를 적용한다. 목표함수는 특정 상태 s에 대한 가치함수로 정의할 수 있다. 가치함수를 미분하여 나온 결과를 정리하는 것이 Policy Gradient Theorem이다.

![](https://cdn-images-1.medium.com/max/800/1*qySDorYr55KgVJ6H3bu_6Q.png)

이 결과를 기댓값의 형태로 정리하고 상태 분포 $$ d_{\pi_\theta}(s) $$ 를 샘플링으로 대체해 식에서 제거하면 다음과 같은 update 식을 얻을 수 있다.

![](https://cdn-images-1.medium.com/max/800/1*zjEh737KfmDUzNECjW4e4w.png)

현재 에이전트는 정책만 가지고 있고 가치함수 혹은 큐함수를 갖고 있지 않다. 그러므로 $$ R(\tau) $$를 어떻게 근사할지가 문제가 된다. 가장 간단한 REINFORCE 알고리즘에서는 이를 반환값으로 대체한다. 에피소드가 끝날 때까지 에피소드 동안 지나온 상태에 대한 각각의 반환값을 구할 수 있으므로, 이를 통해 정책을 업데이트할 수 있다. 에피소드마다 실제로 얻은 보상으로 학습하므로 Monte Carlo Policy Gradient라고도 부른다.

## 더 알아보기

현재에는 이보다 더 발전된 형태의 수없이 많은 강화학습 알고리즘이 있다. 하지만 큐함수를 직접 사용하는 형태와 Policy를 직접 업데이트하는 큰 분류는 동일하다. 다음 포스트에서는 Deep SARSA와 달리 Q-learning의 큐함수 업데이트와 인공신경망, 그리고 experience-replay 메모리를 사용한 DQN, 그리고 REINFOCE의 단점을 보완한 또다른 Policy Gradient 메소드인 Actor-Critic 알고리즘에 대해 더 자세히 알아보기로 하자. 

## Reference

- http://www.modulabs.co.kr/RL_library/2621
- https://hijigoo.github.io/reinforcement%20learning/2019/02/28/rl-dynamic-programming/
- 파이썬과 케라스로 배우는 강화학습, 위키북스


<script>
var disqus_config = function () {
                this.page.url = '{{ site.url }}{{ page.url }}';  // Replace PAGE_URL with your page's canonical URL variable
                this.page.identifier = '{{ page.title }}-{{ page.date }}'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
</script>
