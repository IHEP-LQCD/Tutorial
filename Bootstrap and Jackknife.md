# Notations

#### 1.0

We talk about a random variable $\xi$ with its unknown probability
distribution function $F(\xi)$. Sample $\xi$ with replacement to get a
sample with a size of $n$, $$X = \{ x_1, x_2, ..., x_n \} .$$ Define
statistic $\theta$ as functional of $F(\xi)$, and an estimator
$\hat{\theta}$ can be defined as function of $X$ with parameter $n$,
$$\hat{\theta} = f(X;n) .$$ Its bias and standard error are defined
respectively as, $$\delta(\hat{\theta}) = E(\hat{\theta}) - \theta,$$
$$\sigma(\hat{\theta}) = \sqrt{D(\hat{\theta})},$$ where
$E(\hat{\theta})$ and $D(\hat{\theta})$ are the expectation and
variation of $\hat{\theta}$ respectively.

#### 1.1

Resample $X$ with replacement $b$ times (keeping its size $n$) to get
Bootstrap samples $X_B^{(i)}$ ($i = 1, 2, ..., b$), which are
independently and identically distributed and subject to uniform
distribution on $X$. We derive $\hat{\theta}_B$ of a number of $b$ from
such Bootstrap samples, $$\hat{\theta}_B^{(i)} = f(X_B^{(i)};n) .$$

#### 1.2

Jackknife samples $X_J^{(j)}$ ($j = 1, 2, ..., n$) are defined the same
as $X$, but leave $x_j$ out. We derive $\hat{\theta}_J$ of a number of
$n$ from such Jackknife samples,
$$\hat{\theta}_J^{(j)} = f(X_J^{(j)};n-1).$$

# Bootstrap and Jackknife Estimation

#### 2.1

Bootstrap estimators of bias and standard error are respectively,
$$\hat{\delta}_B(\hat{\theta}) = \overline{\hat{\theta}_B} - \hat{\theta},$$
$$\hat{\sigma}_B(\hat{\theta}) = \sqrt{\frac{1}{b-1}\sum\limits _{i=1}^b (\hat{\theta}_B^{(i)} - \overline{\hat{\theta}_B})^2},$$
where,
$$\overline{\hat{\theta}_B} = \frac{1}{b}\sum\limits _{i=1}^b \hat{\theta}_B^{(i)}.$$

#### 2.2

Jackknife estimators of bias and standard error are respectively,
$$\hat{\delta}_J(\hat{\theta}) = (n-1)(\overline{\hat{\theta}_J} - \hat{\theta}),$$
$$\hat{\sigma}_J(\hat{\theta}) = \sqrt{\frac{n-1}{n}\sum\limits _{j=1}^n (\hat{\theta}_J^{(j)}-\overline{\hat{\theta}_J})^2},$$
where,
$$\overline{\hat{\theta}_J} = \frac{1}{n}\sum\limits _{j=1}^n \hat{\theta}_J^{(j)}.$$

#### 2.3

Jackknife estimator of Bootstrap estimator of standard error
(Jackknife-after-Bootstrap) is,
$$\hat{\sigma}_J(\hat{\sigma}_B) = \sqrt{\frac{n-1}{n}\sum\limits _{j=1}^n (\hat{\sigma}_B^{(j)}-\overline{\hat{\sigma}_B})^2},$$
where,
$$\overline{\hat{\sigma}_B} = \frac{1}{n}\sum\limits _{j=1}^n \hat{\sigma}_B^{(j)},$$
with,
$$\hat{\sigma}_B^{(j)} = \sqrt{\frac{1}{b_j -1}\sum\limits _{i=1}^b (\hat{\theta}_B^{(i)}\Delta_j^{(i)}-\overline{\hat{\theta}_B \Delta_j})^2}$$
where,
$$\overline{\hat{\theta}_B \Delta_j} = \frac{1}{b_j} \sum\limits _{i=1}^b \hat{\theta}_B^{(i)}\Delta_j^{(i)},$$
with, $$b_j = \sum\limits _{i=1}^b \Delta_j^{(i)},$$ and,
$$\Delta_j^{(i)} = 1,\ \mathrm{if}\ x_j \notin X_B^{(i)};\quad \Delta_j^{(i)} = 0,\ \mathrm{otherwise}.$$

# Bootstrap Confidence Intervals

#### 3.0

A confidence interval $[L,U]$ about statistic $\theta$ with significance
$\alpha$ means, $$P(L \leq \theta \leq U) = 1-\alpha.$$ Bootstrap
confidence intervals $[\hat{L}_B,\hat{U}_B]$ are estimators of $[L,U]$.

#### 3.1

The standard normal Bootstrap confident interval is,
$$\hat{L}_B^N(\theta;\alpha) = \hat{\theta} - Z(1-\alpha/2)\hat{\sigma}_B(\hat{\theta}),$$
$$\hat{U}_B^N(\theta;\alpha) = \hat{\theta} + Z(1-\alpha/2)\hat{\sigma}_B(\hat{\theta}),$$
where $Z = \Phi^{-1}$, and $\Phi$ is the standard normal distribution
function, viz.
$$\Phi(x) = \frac{1}{\sqrt{2\pi}}\int _{-\infty}^x \mathrm{e}^{-t^2/2} \ \mathrm{d}t.$$

#### 3.2

The percentile Bootstrap confidence interval is,
$$\hat{L}_B^P(\theta;\alpha) = \hat{\theta}_B^{[l]},\quad \hat{U}_B^P(\theta;\alpha) = \hat{\theta}_B^{[u]},$$
where $\hat{\theta}_B^{[i]}$ are the ascending sort of
$\hat{\theta}_B^{(i)}$, and,
$$l = (b+1)\alpha/2,\quad u = (b+1)(1-\alpha/2).$$

#### 3.3

The basic Bootstrap confidence interval is,
$$\hat{L}_B^B(\theta;\alpha) = 2\hat{\theta} - \hat{\theta}_B^{[u]},\quad \hat{U}_B^B(\theta;\alpha) = 2\hat{\theta} - \hat{\theta}_B^{[l]}.$$

#### 3.4

The Student Bootstrap confidence interval is,
$$\hat{L}_B^S(\theta;\alpha) = \hat{\theta} - \hat{t}_B^{[u]}\hat{\sigma}_B(\hat{\theta}),\quad \hat{U}_B^S(\theta;\alpha) = \hat{\theta} - \hat{t}_B^{[l]}\hat{\sigma}_B(\hat{\theta}),$$
where $\hat{t}_B^{[i]}$ are ascending sort of $\hat{t}_B^{(i)}$, and,
$$\hat{t}_B^{(i)} = \frac{\hat{\theta}_B^{(i)}-\hat{\theta}}{\hat{\sigma}_B(\hat{\theta}_B^{(i)})},$$
where $\hat{\sigma}_B(\hat{\theta}_B^{(i)})$ should be derived by
re-resampling of $X_B^{(i)}$.

#### 3.5

The Bootstrap confidence interval with acceleration modification (BCa)
is,
$$\hat{L}_B^a(\theta;\alpha) = \hat{\theta}_B^{[\mu]},\quad \hat{U}_B^a(\theta;\alpha) = \hat{\theta}_B^{[\nu]},$$
where,
$$\mu = \Phi \Big( \hat{z}+\frac{\hat{z}+Z(\alpha/2)}{1-(\hat{z}+Z(\alpha/2))\hat{a}} \Big),$$
$$\nu = \Phi \Big( \hat{z}+\frac{\hat{z}+Z(1-\alpha/2)}{1-(\hat{z}+Z(1-\alpha/2))\hat{a}} \Big),$$
with,
$$\hat{z} = Z\Big( \frac{1}{b}\sum\limits _{i=1}^b \Theta(\hat{\theta}-\hat{\theta}_B^{(i)}) \Big),$$
where $\Theta$ is Heaviside step function, and,
$$\hat{a} = -\frac{1}{6} \frac{\sum\limits _{j=1}^n (\hat{\theta}_J^{(j)}-\overline{\hat{\theta}_J})^3}{\big(\sum\limits _{j=1}^n (\hat{\theta}_J^{(j)}-\overline{\hat{\theta}_J})^2\big)^{3/2}}.$$
