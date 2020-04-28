---
title: Partial different equation
categories: study
comments: true
disqus: true
modified: 
tags: [공업수학]
ads: true
image:
  feature:
  teaser: teaser/study001.jpg
  thumb:
sitemap :
  changefreq : daily
  priority : 1.0
---

편미분방정식. 말이 익숙해서 배웠는데 내가 공업수학에서 배우고 까먹은 건줄 알았는데 알고보니 수업에서는 배우지 않았다...
대학원 면접때 공업수학 준비를 못해서 물어볼 줄 예상도 못했고...
다시 공부해야겠다 싶어 책을 들었다!

```
Advanced Engineering Mathematics
저자 Erwin Kreyszig

출판 Wiley

발매 2011.01.01.
```
Kreyszig 책 10판을 참고하였다.

#### 12장 PDE
시작에 앞서 두가지 ODE를 풀어본다.

![두가지 ODE](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/1.jpg)

(1) 은 특성방정식을 이용하여 간단하게 풀 수 있다.
(2) 는 변수분리를 통해 문제를 해결하였다.

![모델링](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/2.jpg)

첫번째로 파동방정식의 모델링이다.
길이 L이고 양 끝이 고정되어 있는 줄이다.

![힘분석](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/3.jpg)

위 그림에서 줄 위의 한 점에 작용하는 힘을 분석한다.
수평방향으로는 변위가 0이므로 가속도 역시 0이 되고 작용하는 알짜힘이 0이된다. 따라서 (1) 식과 같은 결과를 얻을 수 있다.
(2)번 식은 수직방향의 운동방정식이다.
왼쪽항은 좌우로 작용하는 장력의 수직성분의 합이고 오른쪽항은 가속도를 나타낸다.
즉 '알짜힘 = 질량*가속도' 가 된다.

위의 운동방정식을 아래와 같이 정리하면 파동방정식을 얻을 수 있다.

![파동방정식](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/4.jpg)

다음은 위의 편미분 방정식을 구하는 과정이다.

![편미분방정식](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/5.jpg)

먼저 주어진 방정식은 위와 같이 쓸 수 있다. 
그리고 x변수 하나의 공간변수를 가지므로 1차 파동방정식이 된다.

![1차파동방정식](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/6.jpg)

풀이를 시작하기 전에 경계조건과 초기조건을 위와같이 구할 수 있다.
①은 줄의 양 끝이 u=0에 고정되어 있음을 이용한다.
②는 초기조건이다. 함수 f는 초기 deflection, 여기서는 u의 값이라고 할 수 있다.
함수 g는 초기 속도이다. 물론 수직방향성분이다.

![step1](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/7.jpg)

Step1. 변수 분리를 이용한다.(Method of separating variables)
함수 u를 x에 대한 함수 F와 t에 대한 함수 G의 곱으로 나타낸다.
이는 공학문제를 해결하는데 많이 사용되는 방법이며 문제해결을 쉽게해주는 가정이다.
이를 이용하면 아래의 식 ①, ②를 얻을 수 있다.
dot은 시간 t에 대한 미분, prime은 공간변수 x에 대한 미분을 나타낸다.

![step1-2](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/8.jpg)

이 때 ③과 같이 정리한 결과를 상수 k로 놓게 된다.
만약 k가 아니라 x나 t에 대한 식이 되면 변수분리를 한 결과와 모순되기 때문이다.
(오타; If k is variables of t or x, u (x,t) can't be seperated.)

![step1-3](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/9.jpg)

위 결과를 통애 두개의 ODE를 구할 수 있다.

![step2](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/10.jpg)

Step2. 경계조건을 이용해 문제를 해결한다.
먼저 x에 대한 경계조건을 이용해서 쉽게 위의 결과를 얻을 수 있다.

![step2-1](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/11.jpg)

다음은 k값의 부호를 결정한다. k=0일 때의 경우는 쉽게 구할 수 있고 그 결과가 중요하지 않다.
먼저 k값이 0보다 클 때이다.
k값이 0보다 크면 F를 풀면(ODE) 위와같이 결과를 얻을 수 있다. 하지만 경계조건을 적용해보면 그 결과가 모순된다는 것을 알 수 있다.

![step2-2](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/12.jpg)


따라서 k는 음수가 되어야 하고 ③식과 같이 k = -p^2으로 쓸 수 있다.
이 값을 적용하여 F를 풀면(ODE) 위와 같은 일반해를 얻을 수 있다.

![step2-3](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/13.jpg)

그리고 앞에서와 같이 경계조건을 적용하면 p = npi/L을 얻을 수 있고 B=1로 놓으면 정수 n에 대한 함수 F를 구할 수 있다.

![step2-4](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/14.jpg)

같은 방식으로 G역시 구할 수 있다.

![step2-5](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/15.jpg)

④와 ⑤의 결과를 합치면 위의 n에 대한 해를 구할 수 있다.
이를 eigenfunction라 한다.
그리고 eigen values, spectrum 과 같은 값들을 확인 할 수 있다.
또한 nomal mode 라는 것이 있는데 이 결과를 통해 진동하는 선에서 만드는 정상파의 조화모드를 확인 할 수 있다. 이 결과는 일반대학물리학에 자세히 나와있다.

![step3](https://raw.githubusercontent.com/Kyungpyo-Kim/Kyungpyo-Kim.github.io/master/_posts/study/Partial-Differential-Equation/16.jpg)

Step3. 위의 결과는 위의 결과를 퓨리에 급수로 해석한 결과이다.
Step2의 결과는 n에 대한 해이고 모든 n에 대한 해를 합쳐야 일반해라 할 수 있다.
그리고 그 일반해는 퓨리에 급수를 통해 위와같이 보다 직관적으로 해석된다.

특히 ③의 결과를 보면 함수 f는 초기의 deflection이었고 이 값이 서로 반대방향으로 진행하여 겹치는 형태의 결과라는 것을 직관적으로 알 수 있다. 
여기서 f*라는 것은 u(0,t)=0, u(L,t)=0 이 결과를 반영함을 알 수 있다.

#### *끝!*
__(이 글은 제가 공부한 내용을 제가 볼려고 정리한 내용이기 때문에 필요한 기초지식이나 일부 내용들은 생략되었습니다. 혹시나 이글을 보신 분들 중에 궁금하신 점이나 잘못된 부분이 있다면 알려주세요...)__
