---
title: "7_regression"
author: "Joonsung Park"
date: "Sunday, January 11, 2015"
output: html_document
---


```r
roe.it<-read.table(file="./ROE.IT.txt")
head(roe.it)
```

```
##     V1  V2
## 1  ROE  IT
## 2  1.4 100
## 3 52.7 800
## 4   12 700
## 5  2.6 350
## 6  6.2  50
```

#7. 회귀분석


> 경제현상을 분석하는 경우 둘 그 이상의 변수들간의 관계에 관심을 갖습니다.

> 예를 들어， 컴퓨터의 소비량은 가격에 영향을 받는지， 기업의 부도율은 재무 성과와 어떤 관계가 있는지， 그리고 주가는 기업의 순이익에 의해 얼마나 영향을 받는지 등을 알고싶습니다.

> 이렇게 변수와 변수 사여의 관계를 알아보기 위한 통계적 분석방법인 상관분석 (correlation analysis)과 회귀분석(regression analysis) 에 대해서 나가겠습니다.


##1. 회귀분석의 기본개념

###1. 상관분석

> 우선 직관적으로 눈에 어떻게 들어오는지는 봐야 할것 같습니다. 그래서 책에는 

> 두 변수 사이의 관계를 살펴볼 수 있는 가장 기본적인 분석방법은 두 변수의 서로 대응되 는 자료를 좌표평면 위의 점들로 나타내는 산포도(scatter plot)를 그려보는 것이 나와있습니다.

> 기업의 정보기술투자와 기업의 수익성이 서로 어떻게 관련이 있는지 알아보기 위해 우리나라 상장기업들에서 임의로 추출한 40개를 대상으로 조사한 자기자본수익률
(ROE)과 사원 1 인당 IT자본(단위 : 만 원)에 대한 자료가 있습니다.


여기서 실행되게 하려다가 결국
캡쳐를 했습니다.ㅠ.ㅠ

테이블
![table](http://postfiles7.naver.net/20150111_38/swatpjs_1420964972020MEHKv_PNG/table7-1.png?type=w2)

산포도
![fig](http://postfiles2.naver.net/20150111_129/swatpjs_1420964971828CTwzD_PNG/fig7-1.png?type=w2)


우리기업의 경우 정보기술투자와 기업의 수 익성 간에 어느 정도 정의 상관관계가 있다는 사실을 보여줍니다. 이와 같이 산포도를 이용하여 두 변수 사이의 관계를 개략적으로 알 수 있으나， 보다 정확한 관계를 파악하기 위해서는 두 변수의 상호 의존관계를 수치로 나타내줄 수 있는 통계량이 필요한데 , 이를 상관계수(correlation coefficient)라고 하며 , 이 상관계수를 이용하여 두 변수의 의존관계를 분석하는 것이 상관분석입니다.

**<상관계수>**

X와 Y의 상관계수는 $\rho$로 표시됩니다.

$$
\rho = \frac{Cov(X,Y)}{\sigma_x\sigma_y}
$$

로 정의 됩니다.

여기서 $Cov(X,Y)$는 X와 Y의 공분산,
$\sigma$는 표준편차를 나타냅니다.

상관계수는 -1 <= $\rho$ <= 1 의 조건을 만족하며,
$\rho=0$는 두 확률 변수 X와 Y간의 선형 상관관계가 없음을
$\rho=1$은 완전한 정의 상관관계가 존재함을
$\rho=0$는 완전한 역의 상관관계가 존재함을 나타냅니다.


![co](http://www.microbiologybytes.com/maths/graphics/correlation.gif.pagespeed.ce.mgtDA_FUzE.gif)




![예제](http://postfiles13.naver.net/20150111_236/swatpjs_1420965786105UC46l_PNG/ex7-1.png?type=w2)



상관계수는 두 변수 사이에 어느 정도의 관계가 존재하는지를 정확히 나타내줄 수는 있으나 두 변수 간에 구체적으로 어떤 함수 관계를 갖는지는 나타내주지 못한다. 그런데 우리는 두 변수 간의 관계식을 파악하여 우리가 알고 있는 변수의 값으로부터 다른 변수의 값을 예측할 필요가 종종 있습니다. 이런 경우에 우리가 적용할 수 있는 통계적 분석방법 이 회귀분석 이랍니다.



###2.회귀모형(p.451)

> 관심의 대상이 되며, 다른 변수에 영향을 받는 변수는 종속변수, 내생변수

> 종속변수에 영향을 주는 변수는 독립변수, 외생변수 라고 합니다.

> 회귀모형
종속 변수와 독립 변수의 관계를 나타내주는 함수형태


> 가장 원초적인 단위로 예를 들자면, 주식 가격이 종속변수라면, 독립변수는 주식을 사고 파는 행위 라고 할수 있겠습니다.

> 금리가 아무리 오르고 환율이 아무리 변해도
> 모든 시장 참여자가 거래를 안한다면
> 주가는 움직이지 않을테니요.


> 단순회귀모형과 다중회귀 모형의 구분은 독립 변수의 수입니다.

> 이들 간의 관계는
> 함수적 의존관계(함수)
> 확률적 의존관계(확률)
> 두가지로 나눠 볼 수 있습니다.

```r
함수적 의존관계
독립 변수의 값을 알면
종속변수의 값을 정확하게 알수 있는 관계
```

$$
y = f(x) = \beta_0 + \beta_1x
$$

![fig](http://postfiles4.naver.net/20150111_227/swatpjs_1420968196844SQLnA_PNG/fig7-4.png?type=w2)


-----------------------------------

##2. 회귀 모형과 기본가정

###1. 회귀모형

회귀분석의 중요한 목적 중 하나는 변수들 간의 관계를 모형화하는 것이지만， 전형적인 경제 응용문제에서 변수들의 근본적인 관계를 나타내는 정확한 함수형태가 이론적으로 제시되지 못하는 경우가 대부분이다. 이런 경우 가장 일반적이면서도 단순한 함수의 형태인 선형모형을 가정하여 두 변수들의 관계를 분석하게 됩니다.



앞의 산포도에서는 기업의 투자액과 수익성의 정의 상관관계를 보았습니다.

이제는 상관관계가 아니라, 값을 구하는 쪽으로 가보겠습니다.

하지만 현실은 딱 떨어지는 것이 없기 때문에
일정 x 가 주어졌을때 y의 기댓값을 구하는 방법이 합리적인 방법이 됩니다.

앞으로 회귀분석식이 어떻게 표현되는지를 
그림으로 나타낸 것입니다.

![그림](http://postfiles13.naver.net/20150111_252/swatpjs_1420968360260YvEoo_PNG/fig7-5.png?type=w2)

![rmfla](https://inspirehep.net/record/1230868/files/fig3D784.png)

y의 기댓값은

$$
E(y_i|x_i)
$$
으로 표현 되구요

독립변수와 조건부 평군과의 함수를 나타내면
$$
E(y_i|x_i) = f(x_i)
$$

$f(x_i)$ 를 모회귀 함수 라고 합니다.

하지만, 산포도에서 본 것 처럼
모든 값들이 회귀**선**상에 있지 않습니다.
기댓값으로부터 어느정도 차이가 있습니다.

만일 이 차이를 평균값이 0인 확률 변수 epsilon으로 나타내면
$
\epsilon_i\ = y_i - E(y_i|x_i) = y_i -(\beta_0+\beta_1x_1)
$ 이 됩니다.


**<단순 회귀 모형>**

$$
y_i = \beta_0=\beta_1x_i+\epsilon_i,\\  
i = 1,\dots,n
\beta_0 는 절편계수
\beta_1 은 기울기계수
\epsilon_i는 평균
값 0을 가지는 관측 불가능한 획률변수로 획률오차항(stochastic elTOr tenn) 또는 확률교란
항(stochastic disturbance)이라고 한다.
$$

이걸 단순하게 표현해 보겠습니다.

**<단순 회귀 모형>**

$$
y = X\beta + \epsilon
$$
$$
y와 \epsilon은 n*1 벡터\\
X는 n*2 벡터\\
\beta 는 2*1 모수벡터 가 됩니다.
$$

![dan](http://postfiles4.naver.net/20150111_131/swatpjs_1420987361679NKWYy_PNG/mat1.png?type=w2)


이제 변수가 많아지는 다중 회귀 모형으로 가보겠습니다.

**< 다중 회귀 모형과 그것의 단순화>**
$$
y_i = \beta_0 + \beta_1x_{i1} + \beta_2x_{i2} + \dots + \beta_Kx_{iK} + \epsilon_i
\\
\\
y = X\beta + \epsilon
$$

![da](http://postfiles14.naver.net/20150111_253/swatpjs_1420987361809DzCOX_PNG/mat2.png?type=w2)

$\beta$인 부분회귀계수에 의해서 결정이 되는데, 
$\beta_0$ 를 제외한 나머지는 중요합니다.

왜냐면, 모든 독립변수의 값이 0 이되는 경우는 거의 불가능 하기 때문입니다.


![fig](http://postfiles5.naver.net/20150111_228/swatpjs_14209686551227fJsR_PNG/fig7-6.png?type=w2)

예를 들어
$$E(y_i|x_{i1},x_{i2})= 2.5 + 0.05x_{i1} - 10x_{i2}
$$
의경우

$x_{i1}$이 1단위 증가하면
기댓값은 0.05 증가하고
$x_{i2}$이 1단위 증가하면
기댓값은 10 감소합니다.

![cov](http://postfiles14.naver.net/20150111_237/swatpjs_1420968910485EiyAE_PNG/cov1.png?type=w2)


###2. 회귀 모형의 기본 가정

회귀모형을 추정하는 추정량이 바람직한 속성을 만족하고 회귀모형을 올바르게 추론하기 위해 요구되는 가정들을 회귀모형을 위한 기본가정(standard assumption)이라고 합니다.

---------------------------------
다섯 번째로 소개된 비다중공선성 가정을 고려하는 독립변수(K)의 수가 2개 이상인 다중회귀모형일 경우에만 해당되며， 다중회귀모형의 경우 비다중공선성에 대한 가정이 추가되는 이유는 최소제곱추정값이 유일하게 존재하지 않는 특수한 경우를 배제하는데 있다. 

극심한 공선성이 존재하여도 최소제곱법에 의해 회귀계수의 추정값을 구할 수 있기는 하지만， 개별 독립변수가 종속변수에 미치는 영향인 개별효과(separate effect)를 추정하는 것은 불가능해진다.
--------------------------------

> 다중공선성 : 독립변수간의 상관관계가 높은 경우 발생
> 비다중공선성 : 각 독립변수는 선형관계가 없다. (다중회귀분석에 추가되는 가정)


**<회귀 모형을 위한 기본 가정>**

$$
* 독립변수 x는 비확률적이다
$$
$$
* 오차항 \epsilon_i는 평균 0을 가지는 확률변수이다.
$$

$$
E(\epsilon_i) = 0
$$


---
$$
* 오차항 \epsilon_i는 모든 x_i에 대하여 동일한 분산을 지닌다.\\
Var(\epsilon_i) = E(\epsilon_i^2) = \sigma^2\\
이 가정을 동분산 또는 균분산 가정이라고 한다.
$$
---
$$
* 오차항 \epsilon_i는 서로 상관되어 있지 않다.\\
E(\epsilon_i, |epsilon_j) = 0, i notsame j \\
이 가정을 비자기상관 가정이라고 한다.
$$
---
$$
* 독립 변수들 간에 완전한 선형 관계가 성립되지 않는다.\\
이 가정을 비다중공산성 가정이라고 한다. 반면에 동시에 0이 아닌\\
c_0, c_1, c_2,\dots,c_k에 대해서
c_0 + c_1x_{1i} + c_2x_{2i} + \dots + c_Kx_Ki = 0 이 성립되는 경우가 있으면\\
독립변수들 사이에 다중공산성이 있다고 한다.
$$
---
$$
*가설 검정을 위해 오차항 \epsilon_i는 정규분포를 따라야 한다.\\
\epsilon_i ~ N(0,\sigma^2)
$$


## 3. 추정 방법

회귀분석의 목적을 달성하기 위해서는 모회귀함수에 주어진 모수들을 정확하게 측정 하는 것이 첫 번째 과제입니다.

여기서는 
* 최소제곱추정량
* 최대우도 추정량
* 일반 적률추정량
세가지를 다룹니다.


###1. 최소 제곱 추정량과 가우스-마코프 정리

> 실제 값과 추정된 값과의 차이가 잔차이며, 이 잔차의 제곱들의 합을 최소화 하는 회귀식을
**최소제곱법**이라고 합니다.

> 르장드르, 가우스, 라플라스 등에 의해 제시되고 발전된이후 회귀모형을 추정하는 가장 일반적인 방법으로 이용되고 있습니다.

```r
K개의 독립변수와 n개의 관측값을 가지는 회귀모형
$$ y = B\beta + \epsilon$$
y와 입실론은 nX1 벡터, X는 n x (K+1)행렬
베타는 (K+1)x1 모수벡터
베타의 가능한 모든 값을 행렬식으로 만든다음 표본 회귀선을 만든다.
```


**<표본회귀선>**
$$
\hat{y_i} = X\hat{\beta}\\
잔차는\\
\epsilon = y - X\hat{\beta}\\
$$



```r
모든 잔차를 최소화하기 위한 최상의 방법은 잔차의 합을 최소화 하는것.

단순히 잔차의 합을 기준으로하면 정의 값과 부의 값이 서로 상쇄되어 직선의 자료에 대한 적합도와는 관계없이 실제 값과 추정값 사이의 편차가 터무니 없이 과소화 됨으로써 좋은 추정값을 구하지 못하게 되기 때문입니다.

이러한 문제를 해결할 수 있는 가장 바랍직한 방법은 잔차 제곱들의 합을 최소화하는 직선을 선택하는 최소제곱법 입니다. 

이는 다음 이차형식의 목적 함수를 최소화 하는 추정량을 구하는 것입니다.

글의 느낌은 왜 우리가 통계학에서 분산을 안쓰고 표준편차를 쓰는지 알려주는 것 같습니다.
```
![448](http://postfiles10.naver.net/20150111_57/swatpjs_1420969258041zlddo_PNG/448.png?type=w2)




$$ X'X\hat{\beta} = X'y $$

따라서 최소제곱 추정량은 다음과 같이 구해 질수 있습니다.

$$ \hat{\beta}_LS = (X'X)^{-1}X'y$$



가우스-마코프(Gauss-Markov) 정리에 의하면 회귀모형에 대한 기본가정이 만족될 경우 최소제곱추정량은 불편추정량이며 최소분산을 가지는 최우수 선형불편추정량(Best Linear Unbiased Estimator, BLUE)이 된다.

![fig](http://postfiles16.naver.net/20150111_47/swatpjs_1420969517994og6H9_PNG/449.png?type=w2)
![fig](http://postfiles3.naver.net/20150111_178/swatpjs_14209695182636lzxA_PNG/fig7-7.png?type=w2)
![fig](http://postfiles1.naver.net/20150111_32/swatpjs_1420969518490OKYJw_PNG/table7-2.png?type=w2)


> 7-5와 7-6 두문제 같이 풀어보고
> 칠판에 써서 생각해 보면 좋을것 같습니다.

<7-5>


```r
Birth.df<-read.table(file="./Birthrate.txt", header=T, row.names=1)
head(Birth.df,n=3)
```

```
##      birth W.act edu.spd cru.div rGDPgw
## 1971  4.54  39.5   8.339     0.3   10.4
## 1972  4.14  39.6   9.927     0.4    6.5
## 1973  4.10  41.5  10.202     0.4   14.8
```

```r
y<-Birth.df$birth
x<-Birth.df[,c(2,3,4,5)]
mls.res<-lsfit(x,y)
ls.print(mls.res)
```

```
## Residual Standard Error=0.456
## R-Square=0.7899
## F-statistic (df=4, 36)=33.85
## p-value=0
## 
##           Estimate Std.Err t-value Pr(>|t|)
## Intercept  10.2108  1.7463  5.8470   0.0000
## W.act      -0.2064  0.0429 -4.8176   0.0000
## edu.spd     0.2277  0.0639  3.5606   0.0011
## cru.div    -0.5813  0.1991 -2.9197   0.0060
## rGDPgw     -0.0067  0.0211 -0.3164   0.7535
```

<7-6>


```r
lm.res<-lm(formula=birth~W.act+edu.spd+cru.div+rGDPgw,data=Birth.df)
summary(lm.res)
```

```
## 
## Call:
## lm(formula = birth ~ W.act + edu.spd + cru.div + rGDPgw, data = Birth.df)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -1.1522 -0.2219  0.0734  0.3066  0.8287 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 10.21078    1.74633    5.85  1.1e-06 ***
## W.act       -0.20644    0.04285   -4.82  2.6e-05 ***
## edu.spd      0.22769    0.06395    3.56   0.0011 ** 
## cru.div     -0.58127    0.19909   -2.92   0.0060 ** 
## rGDPgw      -0.00667    0.02107   -0.32   0.7535    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.456 on 36 degrees of freedom
## Multiple R-squared:  0.79,	Adjusted R-squared:  0.767 
## F-statistic: 33.8 on 4 and 36 DF,  p-value: 9.64e-12
```





###2. 최우 추정량

```r
회귀모형의 계수들을 추정하는 다른 중요한 방법으로 최우법(rnaximum likelihood method) 을 들 수 있다. 비선형회귀모형의 계수들을 추정히는 데 그 중요성이 더욱 부각되는 이기법은 1920년대 초 피셔(R. A. Fisher)에 의해 소개되었으며， 1940년대 이후 그 이론이 본격적으로 이용되기 시작하여 오늘날 최소제곱법과 함께 중요한 추정기법이 되었다. 최소제곱법은 회귀계수 (베타 값들)의 추정값만을 제공하고 오차항의 분산 (시그마제곱) 의 추정값을 제공하지 못하였으나， 최우법은 이 추정값까지 제공해주는 장점이 있다. 반면에 최소제곱추정량이 바람직한 속성을 지니기 위해서 회귀모형을 위한 기본가정중 오7-1항의 분포에 대한 가정은 필요 없는데 반해 최우법을 사용하기 위해서는 오차항의 분포에 대한 기본가정이 요구되며 일반적으로 정규분포를 가정한다.
```
<정규분포 가정>이 필요합니다.

$$
\epsilon_i ~ N(0,\sigma^2)  (i=1,\dots,n)
$$

![eps](http://postfiles12.naver.net/20150111_139/swatpjs_1420987361529wjrQr_PNG/eps.png?type=w2)

$$
lnL = -\frac{n}{2}ln2\pi-\frac{n}{2}ln\sigma^2-\frac{1}{2\sigma^2}(y-X\beta)'(y-X\beta)
$$
가 되며 최 우주추정값은 다음의 극대화 문제의 해로부터 얻을 수 있다.

$$
\max(lnL(\beta,\sigma^2),{\beta\sigma^2})
$$

여기서 $\sigma^2$를 안다고 가정하면
등식의 처음 두 항을 상수로 취급할 수 있고
$-\frac{1}{2\sigma^2}$는 마이너스 값이니깐
로그L을 극대화 시키는 해를 구하는 것은
아래의 극소화 문제의 값을 구하는 것과 같다.

$$
min(y-X\beta)'(y-X\beta)
$$

![woo](http://postfiles1.naver.net/20150112_144/swatpjs_1420989562353wovB9_PNG/woo.png?type=w2)

![t7-4](http://postfiles9.naver.net/20150112_216/swatpjs_1420989561959YGYzg_PNG/table7-4.png?type=w2)

예제 7-8이 7-5와 관련이 있습니다.


##3. 일반적률 추정량

> 앞의 최우주추정량은 회구모형의 모수를 추정하기 위해 오차항의 분포에 대한 가정이 추가로 요구되었습니다.

> 하지만 경제이론은 경제자료의 분포에 대한 정보를 거의 제공하지 않습니다.

```r
그만한 내용 없이 만들어 지기 때문이 아닌가 싶습니다.
```

> 그래서 오차항의 분포 함수에 대한 가정없이 적률 조건만을 이용하여 회귀모형의 모수를 추정할 수 있는 일반 적률추정량은 좋은 대안으로 여겨졌답니다.

> 1982년 한센에 의해 발견된 이후 오차항의 분포 함수를 모르는 경우나 회귀모형의 가정이 충족되지 못하여 최우주 추정량을 적용할 수 없을때 준비모수추정방법으로 다양하게 사용되고 있습니다.

> 일반적률추정량은 최우주추정량보다 **로버스트**하며 복잡한 회귀모형의 경우 최우주추정량에 비해 계산이 용이하다는 장점이 있지만, 덜 효율적일 수도 있습니다.

> 적률법
> 적률이란 확률변수 $X$가 있을때 $X^k$의 기댓값 입니다.

> 확률변수 X의 k차 적률은 $E(X^k)$가 됩니다.
> 예로 $\mu = E(y)$ 를 추정한다고 가정합니다.

$$ E(y) - \mu = 0 $$

> 우리가 사용하게 되는 자료는 표본자료 입니다.
> 모집단 적률의 특성은 표본의 크기가 커지면 표본적률에 의해 거의 동일하게 복제될 수 있다는 유사성 원리를 적용하여 몾ㅂ단 적률조건 대신 표본적률조건을 만족하는 추정치가 됩니다.

$$ 표본적률 조건 \\ \frac{1}{n}\sum_{i=1}^{n} y_i - \mu = 0$$

> 이 추정 방법은 적률 추정량이라고 합니다.

> K개 독립변수와 n개의 관측값을 가지는 예시를 듭니다.

$$ y_i = x_i\beta + \epsilon_i $$

>이 회귀모형의 기본가정에 의하면 $E(\epsilon_i|\x_i) = 0$ 이 만족되며, 다음 직교조건이 성립된다.

$$ Cov(x_i,\epsilon_i)=0 or E(x_i\epsilon_i)=0$$

> 이 항들을 모두 더하면 모집단 적률 조건이 되며, 

$$ E[X(y-X\beta)]=0$$

> 이에 대응하는 표본 적률조건은 

$$ \frac{1}{n}\sum_{i=1}^{n} x_i(y_i - x_i\beta) = 0 $$

> 으로 등식에서 모수 $\beta$의 해를 구하면
> 적률 추정량이 나옵니다.

$$ \hat(\beta)_m = [\sum_{i=1}^{n} x_i' x_i]^-1 \sum_{i=1}^{n} x_i' y_i$$

> 회귀 모형의 기본가정 $E(\epsilon_i|x_i) = 0이 충족되는 경우 적률 추정량 $\hat(\beta)_m$은 최소제곱추정량과 동일함을 알 수 있다.

> 그런데 적률 추정법은 추정해야 하는 모수의 수와 적률 조건이 일치할때 구할 수 있다.
> 만약 추정해야 하는 모수의 수보다 적률조건이 더 많게 되면 등식 시스템은 대수적으로 초과식별이 되어 $\beta$의 해를 구할수 없게 된다.

> 하지만 등식시스템이 이런 초과식별 문제에도 불구하고 $\beta$의 해에 근접하는 추정치를 구하려는 접근법이 일반 적률 추정법이다.

**일반적률추정량**

> 회귀모형의 모수의 수 K+1을 $K^~$로 표현하는데, 회귀모형에서 $K^~$X1벡터의 $\beta\를 추정하여, q개의 모집단 적률조건을 가정한다. 다음과 같이.

![234](http://postfiles7.naver.net/20150112_54/swatpjs_1421047313977bltED_PNG/460.png?type=w2)

![ㅅ뮤디](http://postfiles14.naver.net/20150112_125/swatpjs_1421046969428FhnQy_PNG/table7-5.png?type=w2)

> 예제 7-9가 7-5에 이어집니다.

install.packages("gmm")
library("gmm")

attach(Birth.df)
equation<-birth~W.act+edu.spd+cru.div+rGDPgw
ist.variables<-~W.act+edu.spd+cru.div+rGDPgw
gmm.res<-gmm(g=equation, x=ist.variables)
detach(Birth.df)
summary(gmm.res)