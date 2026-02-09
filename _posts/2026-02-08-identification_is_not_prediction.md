---
layout: post
title:  "Identification Is Not Prediction"
date:   2026-02-08
last_modified_at: 2026-02-08
categories: [econometrics]
pinned: false
---

&nbsp;

If you asked most data scientists if a model that has good fit is necessarily causal, I believe most would correctly answer "no." However, if you instead asked if a model that we know we have identified the parameters for (that is, we can give the parameters a causal interpretation) must have good fit, I think you'd probably have a decent proportion incorrectly answer "yes." The thing is though, its pretty basic econometric manipulation to show that the answer is a resounding "No!" This topic will be the focus of my unc rant today.

&nbsp;

## A Perfectly Identified Model Can Have Bad Predictive Performance

To begin, given a feature matrix $$X$$ and outcome $$y$$, suppose we believe an appropriate model is as follows:

$$y = f(X; \theta)\beta + \epsilon$$

where $$\beta$$ and $$\theta$$ are parameters, $$\epsilon$$ is the error term, and $$f$$ is some function. The only assumptions we will make about $$f$$ are that it's continuous and differentiable over the entirety of $$X$$ space. We will also assume that $$\text{var}[\epsilon] = \sigma^2$$ and $$\text{var}[f(X; \beta)] = \Sigma_f$$.

&nbsp;

When we want to think about model fit, there's many metrics we can consider. For the purposes of this post, we'll focus on the $$R^2$$.

&nbsp;

One can show that for our model above the $$R^2$$ will be equal to the following:

$$R^2 = \frac{\beta' \Sigma_f \beta}{\beta' \Sigma_f \beta + \sigma^2}$$

An important observation: This is the $$R^2$$ we would obtain if we *knew* the true values of $$\beta$$, $$\theta$$, and $$\sigma^2$$. Moreover, note that $$\partial R^2/\partial \sigma^2 < 0$$. Let this sink in: Even if this model is the true data generating process for $$y$$, and even if we knew the true values of all parameters -- meaning we know exactly how $$X$$ causes $$y$$ -- if $$\sigma^2$$ is "large", the $$R^2$$ could be tiny! That we know the true parameters does not mean we have a model that has a good fit!

&nbsp;

Of course, in reality we do not know the true parameters. And so, we must estimate them with data. The sample analogue for the above is below:

$$\hat{R}^2 = \frac{\hat{\beta}' \hat{\Sigma}_f \hat{\beta}}{\hat{\beta}' \hat{\Sigma}_f \hat{\beta} + \hat{\sigma^2}}$$

Where $$\hat{\beta}$$ and $$\hat{\theta}$$ are computed via some estimator; $$\hat{\Sigma}_f$$ is computed by plugging in $$X$$ and $$\hat{\theta}$$ into $f$ and taking the variance; and $$\sigma^2$$ is computed as the variance of the residuals. The point above though still lingers: Even if our estimator is able to properly identify $$\beta$$ and $$\theta$$, if $$\hat{\sigma}^2$$ is relatively large, we will find ourselves with poor fit. **Just as model fit does not have any bearing for identification, identification has no bearing on model fit.**

&nbsp;

We can also extend this reasoning to out-of-sample $$R^2$$. Unlike the standard in-sample $$R^2$$, there is no canonical out-of-sample definition. I prefer the formulation advocated by Hawinkel, Waegeman, and Maere 2023:

$$R^2_{oss} = 1 - \frac{E_{oos}[(y_{oos}-\beta f(X_{oos};\theta))^2]}{[(n+1)/n]^{-1}\text{var}[y]}$$

Letting $$\eta = (n+1)/n$$, the sample analouge for this can be written as follows:

$$
\begin{aligned}
\hat{R}^2_{oos} = \frac{\eta(\hat{\beta}'\hat{\Sigma}_f\hat{\beta} + \hat{\sigma}^2) - \hat{\sigma}^2_{oos}}{\eta(\hat{\beta}'\hat{\Sigma}_f\hat{\beta} + \hat{\sigma}^2)}
\end{aligned}
$$ 

A little quotient rule will reveal a seemingly counterintuitive fact: $$\partial \hat{R}^2_{oos}/\partial \hat{\sigma}^2 > 0$$.  Taking this to mean that more noise improves our forecast though is a misinterpretation. From the out-of-sample perspective, $$\hat{\sigma}^2$$ is the comparison point. It represents the how variable the model was in-sample -- the point of $${R}^2_{oos}$$ is to see how the variance of forecasts compares. Moreover, and more importantly, $$\partial \hat{R}^2_{oos}/\partial \hat{\sigma}^2_{oos} < 0$$. Meaning, the more random noise we have in new data, the less predictive the model will be - even if, again, we have identified the parameters perfectly!

&nbsp;

## Even More Upsetting: An Unidentified Model Can Mislead on True Underlying Predictive Performance

Suppose the true data generating process is given by the following:

$$
\begin{aligned}
y = \beta f(X; \theta) + \gamma g(C; \psi) + \epsilon
\end{aligned}
$$

Assume both $$f(X; \theta)$$ and $$g(C; \psi)$$ are exogenous -- that is, $$E[\epsilon | f]$$ and $$E[\epsilon | g]$$ are equal to 0. We will also assume $$\text{cov}(f, g) \ne 0$$. Additoinally, assume $$\text{var}(\epsilon) = \sigma^2$$.

&nbsp;

Say it is not possible to observe $$g(C; \psi)$$. Therefore,
we choose to estimate the following:

$$
\begin{aligned}
y = \beta f(X; \theta) + \omega
\end{aligned}
$$

where $$\omega \equiv \gamma g(C; \psi) + \epsilon$$. 

&nbsp;

Assume the estimator we use to estimate $$\beta$$ and $$\theta$$, like OLS, aims to minimize the sum of squared errors (SSE). If it were the case that $$\hat{\beta} = \beta$$ and $$\hat{\theta} = \theta$$ minimized SSE, then such an estimator would identify $$\beta$$ and $$\theta$$. However, from the assumptions above, we know that in the context of the model we estimate, $$X$$ is not exogenous. Hence, we are likely to have $$E[\hat{\beta}] \ne \beta$$ and $$E[\hat{\theta}] \ne \theta$$. But, by the nature of the estimator, these estimates will minimize SSE. Consequently, we must have

$$
\begin{aligned}
SSE(\hat{\beta}, \hat{\theta}) \leq SSE(\beta, \theta)
\end{aligned}
$$

for the model $$y = \beta f(X; \theta) + \omega$$. By definition,

$$
\begin{aligned}
R^2 = 1 - \frac{SSE}{SST}.
\end{aligned}
$$

It is easy to see that $$R^2$$ is decreasing in SSE. Therefore, the estimated model (which fails to identify $$\beta$$ and $$\theta$$ properly) will have an $$R^2$$ that is at least as good as the $$R^2$$ that would be obtained for the model $$y = \beta f(X; \theta) + \omega$$ if we could identify the parameters. In essence, the failure to identify the parameters in the restricted model means that we will overestimate the predictive ability of $$f(X; \theta)$$. 

&nbsp;

Make no mistake, though, this in context of the restricted model -- not in the context of the true model. That is, if we were able to include $$g(C; \psi)$$ and estimate the true data generating process, that $$R^2$$ would be superior. To reason this, it is a well known property that the inclusion of another feature either increases the $$R^2$$ or leaves it unchanged at worst. Therefore, whatever the $$R^2$$ is that we obtain from the unidentified restricted model, if we could include the omitted feature, then the $$R^2$$ must increase or remain unchanged at worst - whether or not the inclusion of the feature results in identification or not. 

&nbsp;

This reveals a fact that, although not hard to derive, seems to be rarely explicitly stated and acknowledged: Failure to identify the parameters of a given model can mean failure to identify the $$R^2$$. More specifically, in a case such as the above where certain features must be omitted, absent proper identification techniques, in the context of the restricted model (i.e., the one that omits features), the predictive performance (as judged by the $$R^2$$) of the features we are able to include will be weakly overstated relative to their predictive performance in the same restricted model where we properly identify the parameters.

&nbsp;

The below simulation confirms these findings empirically for the case of a linear model of the form $$y = \beta_0 + \beta_1x + \gamma c + \epsilon$$ where $$\beta_0 = \beta_1 = \gamma = 1$$ and $$\sigma^2 = 1$$ (the default in `rnorm`):

&nbsp;

```
set.seed(123)
n <- 10000
c <- rnorm(n)
x <- 1 + c + rnorm(n)
BETA <- c(1,1) # True Value of Beta
ALPHA <- 1 # True Intercept
X <- cbind(x, c) # True Design Matrix
e <- rnorm(n) # True Error Term
y <- ALPHA + X%*%BETA + e
(t(BETA)%*%var(X)%*%BETA)/(t(BETA)%*%var(X)%*%BETA + var(e)) # True R^2 of True DGP
          [,1]
[1,] 0.8336584
lm(y~X)$coefficients # Estimated coefficients using true model - note the closeness to truth
(Intercept)          Xx          Xc 
  0.9961040   0.9968151   1.0232455 
summary(lm(y~X))$r.squared # Est. R^2 of Est True GDP
[1] 0.8357413
var(x)/(var(x) + var(y-1-x)) # Correct R^2 of Restricted DGP That Excludes C
          [,1]
[1,] 0.4968265
lm(y~x)$coefficients # Estimated coefficients using restricted model - Note the bias for x
(Intercept)           x 
  0.4894001   1.5069476 
summary(lm(y~x))$r.squared # Estimated R^2 of the Restricted DGP
[1] 0.7502936
```

&nbsp;

In this example, the bias that results from the failure to properly identify the impact of $$x$$ on $$y$$ in the context of the restricted model would lead us to believe that $$x$$ truly explains $$75\%$$ of the variation in $$y$$. In reality, $$x$$ in and of itself really only explains roughly half of the variation in $$y$$. The reason our estimated model suggests otherwise is due to the correlation between $$x$$ and $$c$$ (and between $$c$$ and $$y$$). Letting $$X$$ denote the matrix with a constant and $$x$$ as columns and letting $$\beta = [\beta_0, \beta_1]$$, note the OLS estimator for $$\beta$$ for the restricted model will be given by

$$
\begin{aligned}
\hat{\beta} = (X'X)^{-1}Xy
\end{aligned}
$$

Taking expectations (conditional on $$X$$), we have

$$
\begin{aligned}
E[\hat{\beta}] &= E[(X'X)^{-1}X'y] \\
&= (X'X)^{-1}X'(X)E[(X\beta + \gamma C + \epsilon)] \\
&= \beta + \gamma E[(X'X)^{-1}X' C]
\end{aligned}
$$

Let $$\hat{\alpha} = (X'X)^{-1}X'C$$. Note that $$\hat{\alpha}$$ is the OLS estimated coefficient vector we would obtain from regressing $$C$$ on $$X$$. We know $$\gamma = 1$$. Moreover, using the data generating process in the simulation, one can easily show that $$E[\hat{\alpha}] = [-0.5, 0.5]$$. Therefore, $$\gamma E[(X'X)^{-1}X'C] =[-0.5, 0.5]$$. Therefore, for the restricted model we have $$E[\hat{\beta}] = [0.5, 1.5]$$. Effectively, what this means is that because $$x$$ and $$C$$ are correlated, some of $$C$$'s influence "slips in" through $$x$$ even though $$C$$ is excluded. It is for this reason why $$x$$ appears to have more predictive power by itself compared to if we were able to identify $$\beta$$ properly.

&nbsp;

In this sense, it is not necessarily that the $$R^2$$ we obtain from the unidentified model is misleading -- so long as $$x$$ and $$c$$ continue to maintain their relationship, $$x$$ by itself can be decently predictive of $$y$$. However, we can easily imagine that if this relationship broke down that $$x$$'s predictive power would weaken.

&nbsp;

## Concluding Thoughts

It is well known that prediction does not imply identification. But, identification does not imply prediction. Given enough noise in your error, a perfectly identified model can have terrible predictive ability. In turn, a model with good predictive ability can mislead on the true underlying predictive power of a given feature set.

An important implication that arises from this discussion pertains to the nature of the error term. Too often we hand wave away the error term by saying things like "no model is perfect." Some error, of course is just noise. It would be prudent, though, to be explicit about is inside one's error term. Is it truly random noise? I would argue in many cases, probably not. This is a topic in and of its own that I'm currently cooking up something for. So until then, if someone tries to suggest that estimates are wrong simply due to poor predictive power, be sure to tell them that is nonsense: Poor predictive ability tells you nothing about identification!
