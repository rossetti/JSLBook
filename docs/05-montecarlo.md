# Monte Carlo Methods {#mcm}

**[Learning Objectives]{.smallcaps}**

- To be able to perform simple Monte Carlo simulation experiments using the JSL
- To review statistical concepts and apply them to the analysis of simple Monte Carlo simulation experiments
-	To illustrate generating and collecting statistics for Monte Carlo simulation experiments

This chapter illustrates how to use the JSL for simple Monte-Carlo
simulation experiments. The term Monte Carlo generally refers to the set of methods
and techniques that estimate quantities by repeatedly sampling from
models/equations represented in a computer. As such, this terminology is
somewhat synonymous with computer simulation itself. The term Monte
Carlo gets its origin from the Monte Carlo casino in the Principality of
Monaco, where gambling and games of chance are well known. There is no
one Monte Carlo method. Rather there is a collection of algorithms and
techniques. In fact, the ideas of random number generation and random
variate generation previously discussed form the foundation of Monte
Carlo methods.

For the purposes of this chapter, we limit the term Monte Carlo methods
to those techniques for generating and estimating the expected values of
random variables, especially in regards to static simulation. In static
simulation, the notion of time is relatively straightforward with
respect to system dynamics. For a static simulation, time 'ticks' in a
regular pattern and at each 'tick' the state of the system changes (new
observations are produced). 

## Simple Monte Carlo Integration {#ssMC}

In this example, we illustrate one of the fundamental uses of Monte
Carlo methods: estimating the area of a function. Suppose we have some
function, $g(x)$, defined over the range $a \leq x \leq b$ and we want
to evaluate the integral:

$$ \theta = \int\limits_{a}^{b} g(x) \mathrm{d}x$$


Monte Carlo methods allow us to evaluate this integral by couching the
problem as an estimation problem. It turns out that the problem can be
translated into estimating the expected value of a well-chosen random
variable. While a number of different choices for the random variable
exist, we will pick one of the simplest for illustrative purposes.
Define $Y$ as follows with $X \sim U(a,b)$:

\begin{equation}
Y = \left(b-a\right)g(X)
(\#eq:ch3Y)
\end{equation}

Notice that $Y$ is defined in terms of $g(X)$, which is also a random
variable. Because a function of a random variable is also a random
variable, this makes $Y$ a random variable . Thus, the expectation of
$Y$ can be computed as follows:

\begin{equation}
E\lbrack Y \rbrack = \left(b-a\right)E\lbrack g(X)\rbrack
(\#eq:mcEY)
\end{equation}

Now, let us derive,$E\lbrack g(X) \rbrack$. By definition,

$$ E_{X}\lbrack g(x) \rbrack = \int\limits_{a}^{b} g(x)f_{X}(x)\mathrm{d}x$$

And, the probability density function for a $X \sim U(a,b)$ random
variable is:

$$f_{X}(x) =
\begin{cases}
\frac{1}{b-a} & a \leq x \leq b\\
0   & \text{otherwise}
\end{cases}$$\
Therefore,

\begin{equation}
E_{X}\lbrack g(x) \rbrack  = \int\limits_{a}^{b} g(x)f_{X}(x)\mathrm{d}x = \int\limits_{a}^{b} g(x)\frac{1}{b-a}\mathrm{d}x
\end{equation}

Substituting into Equation \@ref(eq:mcEY), yields,

$$\begin{aligned}
E\lbrack Y \rbrack & = E\lbrack \left(b-a\right)g(X) \rbrack = \left(b-a\right)E\lbrack g(X) \rbrack\\
      & =  \left(b-a\right)\int\limits_{a}^{b} g(x)\frac{1}{b-a}\mathrm{d}x \\
      & = \int\limits_{a}^{b} g(x)\mathrm{d}x = \theta\end{aligned}$$

Therefore, by estimating the expected value of $Y$, we can estimate the
desired integral. From basic statistics, we know that a good estimator
for $E\lbrack Y \rbrack$ is the sample average of observations of $Y$. Let $Y_{1}, Y_{2},...Y_{n}$ be a
random sample of observations of $Y$. Let $X_{i}$ be the $i^{th}$
observation of $X$. Substituting each $X_{i}$ into
Equation \@ref(eq:ch3Y) yields the $i^{th}$ observation of $Y$,

$$Y_{i} = \left(b-a\right)g(X_{i})$$

Then, the sample average of is:

$$\begin{aligned}
\bar{Y}(n) & = \frac{1}{n}\sum\limits_{i=1}^{n} Y_{i} = \left(b-a\right)\frac{1}{n}\sum\limits_{i=1}^{n}\left(b-a\right)g(X_{i})\\
  & = \left(b-a\right)\frac{1}{n}\sum\limits_{i=1}^{n}g(X_{i})\\\end{aligned}$$

where $X_{i} \sim U(a,b)$. Thus, by simply generating
$X_{i} \sim U(a,b)$, plugging the $X_{i}$ into the function of interest,
$g(x)$, taking the average over the values and multiplying by
$\left(b-a\right)$, we can estimate the integral. This works for any
integral and it works for multi-dimensional integrals. While this
discussion is based on a single valued function, the theory scales to
multi-dimensional integration through the use of multi-variate
distributions. 

Suppose that we want to estimate
the area under $f(x) = x^{\frac{1}{2}}$ over the range from $1$ to $4$. That is, we want to evaluate the integral:

$$\theta = \int\limits_{1}^{4} x^{\frac{1}{2}}\mathrm{d}x = \dfrac{14}{3}=4.6\bar{6}$$

According to the previously presented theory, we need to generate
$X_i \sim U(1,4)$ and then compute $\bar{Y}$, where
$Y_i = (4-1)\sqrt{X{_i}}= 3\sqrt{X{_i}}$.  In addition, for this simple example,
we can easily check if our Monte Carlo approach is working because we
know the true area.

```java
double a = 1.0;
double b = 4.0;
UniformRV ucdf = new UniformRV(a, b);
Statistic stat = new Statistic("Area Estimator");
int n = 100; // sample size
for(int i=1;i<=n;i++){
	double x = ucdf.getValue();
	double gx = Math.sqrt(x);
	double y = (b-a)*gx;
	stat.collect(y);
}
System.out.printf("True Area = %10.3f\n", 14.0/3.0);
System.out.printf("Area estimate = %10.3f\n", stat.getAverage());
System.out.println("Confidence Interval");
System.out.println(stat.getConfidenceInterval());
```

```
True Area =      4.667
Area estimate =      4.781
Confidence Interval
[4.608646560421988, 4.952515649272401]
```
Because confidence intervals may form the basis for decision making, you
can use the confidence interval half-width in determining the sample
size. A review of these and other statistical concepts will be the focus of the next section.

## Review of Statistical Concepts {#ch5-StatReview}

The simulation models that have been illustrated have
estimated the quantity of interest with a point estimate (i.e.
$\bar{X}$). For example, in the previous section, we can easily compute the true area under the curve as $4.6\bar{6}$; however, the point estimate returned by the simulation was a value of 4.781, based on 100 samples. Thus, there is
sampling error in our estimate. The key question examined in this
section is how to control the sampling error in a simulation experiment.
The approach that we will take is to determine the number of samples so
that we can have high confidence in our point estimate. In order to
address these issues, we need to review some basic statistical concepts in order to understand the meaning of sampling error.

### Point Estimates and Confidence Intervals {#ch5-point-est}

Let $x_{i}$ represent the $i^{th}$ observations in a sample, $x_{1}, x_{2},...x_{n}$ of size $n$. Represent the random variables in the sample as $X_{1}, X_{2},...X_{n}$. The
random variables form a random sample, if 1) the $X_{i}$ are independent
random variables and 2) every $X_{i}$ has the same probability
distribution. Let's assume that these two assumptions have been
established. Denote the unknown cumulative distribution function of $X$
as $F(x)$ and define the unknown expected value and variance of $X$ with
$E[X] = \mu$ and $Var[X] = \sigma^2$, respectively.

A statistic is any function of the random variables in a sample. Any
statistic that is used to estimate an unknown quantity based on the
sample is called an estimator. What would be a good estimator for the
quantity $E[X] = \mu$? Without going into the details of the meaning of
statistical goodness, one should remember that the sample average is
generally a good estimator for $E[X] = \mu$. Define the sample average
as follows: 

\begin{equation}
\bar{X} = \frac{1}{n}\sum_{i=1}^{n}X_i
  (\#eq:sampleAvg)
\end{equation}

Notice that $\bar{X}$ is a function of the
random variables in the sample and therefore it is also a random
variable. This makes $\bar{X}$ a statistic, since it is a function of the
random variables in the sample.

Any random variable has a corresponding probability distribution. The
probability distribution associated with a statistic is called its
sampling distribution. The sampling distribution of a statistic can be
used to form a confidence interval on the point estimate associated with
the statistic. The point estimate is simply the value obtained from the
statistic once the data has been realized. The point estimate for the
sample average is computed from the sample:

$$\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i$$ 

A confidence interval expresses a degree of certainty associated with a
point estimate. A specific confidence interval does not imply that the
parameter $\mu$ is inside the interval. In fact, the true parameter is
either in the interval or it is not within the interval. Instead you
should think about the confidence level $1-\alpha$ as an assurance about
the procedure used to compute the interval. That is, a confidence
interval procedure ensures that if a large number of confidence
intervals are computed each based on $n$ samples, then the proportion of
the confidence intervals that actually contain the true value of $\mu$
should be close to $1-\alpha$. The value $\alpha$ represents risk that
the confidence interval procedure will produce a specific interval that
does not contain the true parameter value. Any one particular confidence
interval will either contain the true parameter of interest or it will
not. Since you do not know the true value, you can use the confidence
interval to assess the risk of making a bad decision based on the point
estimate. You can be confident in your decision making or conversely
know that you are taking a risk of $\alpha$ of making a bad decision.
Think of $\alpha$ as the risk that using the confidence interval
procedure will get you fired. If we know the sampling distribution of
the statistic, then we can form a confidence interval procedure.

Under the assumption that the sample size is large enough such that the
distribution of $\bar{X}$ is normally distributed then you can form an
approximate confidence interval on the point estimate, $\bar{x}$.
Assuming that the sample variance:

\begin{equation}
S^{2}(n) = \frac{1}{n-1}\sum_{i=1}^{n}(X_i-\bar{X})^2
  (\#eq:sampleVar)
\end{equation}

is a good estimator for $Var[X] = \sigma^2$, then a $(1-\alpha)100\%$
two-sided confidence interval estimate for $\mu$ is: 

\begin{equation}
\bar{x} \pm t_{1-(\alpha/2), n-1} \dfrac{s}{\sqrt{n}}
  (\#eq:ci)
\end{equation}

where $t_{p, \nu}$ is the $100p$ percentage
point of the Student t-distribution with $\nu$ degrees of freedom. The spreadsheet 
function T.INV(p, degrees freedom) computes the *left-tailed*
Student-t distribution value for a given probability and degrees of
freedom. That is, $t_{p, \nu} = T.INV(p,\nu)$. The spreadsheet tab labeled *Student-t* in
the spreadsheet that accompanies this chapter illustrates functions related to the Student-t distribution. Thus, in order to compute $t_{1-(\alpha/2), n-1}$, use the following formula:

$$t_{1-(\alpha/2), n-1} = T.INV(1-(\alpha/2),n-1)$$
The T.INV.2T($\alpha$, $\nu$) spreadsheet function directly computes the *upper two-tailed* value.  That is,

$$
t_{1-(\alpha/2), n-1} =T.INV.2T(\alpha, n-1)
$$
Within the statistical package R, this computation is straight-forward by using the $t_{p, n} = qt(p,n)$ function.

$$
t_{p, n} = qt(p,n)
$$

```r
#'qt(0.975, 5)
qt(0.975,5)
```

```
## [1] 2.570582
```

```r
#' set alpha
alpha = 0.05
#'qt(1-(alpha/2, 5)
qt(1-(alpha/2),5)
```

```
## [1] 2.570582
```


### Sample Size Determination {#ch5-SampleSize}

The confidence interval for a point estimator can serve as the basis for
determining how many observations to have in the sample. From Equation (\@ref(eq:ci)), the quantity:

\begin{equation}
h = t_{1-(\alpha/2), n-1} \dfrac{s}{\sqrt{n}}
  (\#eq:hw)
\end{equation}

is called the half-width of the confidence interval. You can place a
bound, $\epsilon$, on the half-width by picking a sample size that satisfies:

\begin{equation}
h = t_{1-(\alpha/2), n-1} \dfrac{s}{\sqrt{n}} \leq \epsilon
  (\#eq:hwBound)
\end{equation}

We call $\epsilon$ the margin of error for the bound.  Unfortunately, $t_{1-(\alpha/2), n-1}$ depends on $n$, and thus
Equation (\@ref(eq:hwBound)) is an iterative equation. That is, you must try different values of $n$ until the condition is satisfied. Within this text, we call this method for determining the sample size the *Student-T iterative method*.

Alternatively, the required sample size can be approximated using the
normal distribution. Solving Equation (\@ref(eq:hwBound)) for $n$ yields:

$$n \geq \left(\dfrac{t_{1-(\alpha/2), n-1} \; s}{\epsilon}\right)^2$$

As $n$ gets large, $t_{1-(\alpha/2), n-1}$ converges to the
$100(1-(\alpha/2))$ percentage point of the standard normal distribution
$z_{1-(\alpha/2)}$. This yields the following approximation:

\begin{equation}
n \geq \left(\dfrac{z_{1-(\alpha/2)} s}{\epsilon}\right)^2
  (\#eq:zSampleSize)
\end{equation}

Within this text, we refer to this method for determining the sample size as the *normal approximation* method. This approximation generally works well for large $n$, say $n > 50$. Both of these methods require an initial value for the standard deviation. 

In order to use either the *normal approximation* method or the *Student-T iterative* method, you must have an initial value for, $s$, the sample standard deviation. The simplest way to get an initial estimate of $s$ is to make a small initial pilot sample
(e.g. $n_0=5$). Given a value for $s$ you can then set a desired bound and
use the formulas. The bound is problem and performance measure dependent
and is under your subjective control. You must determine what bound is
reasonable for your given situation. One thing to remember is that the
bound is squared in the denominator for evaluating $n$. Thus, very small
values of $\epsilon$ can result in very large sample sizes.

***
\BeginKnitrBlock{example}
<span class="example" id="exm:ex2SampleSize"><strong>(\#exm:ex2SampleSize) </strong></span>Suppose we are required to estimate the output from a simulation so that we are 99% confidence that we are within $\pm 0.1$ of the true population
mean. After taking a pilot sample of size $n=10$, we have estimated $s=6$. What is the required sample size?
\EndKnitrBlock{example}

***

We will use Equation (\@ref(eq:zSampleSize)) to determine the sample size requirement. For a 99% confidence interval, we have
$\alpha = 0.01$ and $\alpha/2 = 0.005$. Thus, $z_{1-0.005} = z_{0.995} = 2.576$. Because the margin of error, $\epsilon$ is $0.1$, we have that,

$$n \geq \left(\dfrac{z_{1-(\alpha/2)}\; s}{\epsilon}\right)^2 = \left(\dfrac{2.576 \times 6}{0.1}\right)^2 = 23888.8 \approx 23889$$

If the quantity of interest is a proportion, then a different method can
be used. In particular, a $100\times(1 - \alpha)\%$ large sample two sided 
confidence interval for a proportion, $p$, has the following form:

\begin{equation}
\hat{p} \pm z_{1-(\alpha/2)} \sqrt{\dfrac{\hat{p}(1 - \hat{p})}{n}}
  (\#eq:propCI)
\end{equation}

where $\hat{p}$ is the estimate for $p$. From this, you can determine
the sample size via the following equation:

\begin{equation}
n = \left(\dfrac{z_{1-(\alpha/2)}}{\epsilon}\right)^{2} \hat{p}(1 - \hat{p})
  (\#eq:pSampleSize)
\end{equation}

Again, a pilot run is necessary for obtaining an initial estimate of $\hat{p}$, for use in determining the sample size. If no pilot run is
available, then $\hat{p} =0.5$ is often assumed as a worse case approximation.

If you have more than one performance measure of interest, you can use
these sample size techniques for each of your performance measures and
then use the maximum sample size required across the performance
measures. Now, let's illustrate these methods based on a small Arena simulation.

### Determining the Sample Size for a Monte Carlo Simulation Experiment {#ch5-sample-size}

To facilitate some of the calculations related to determining the sample size for a simulation experiment, I have constructed a spreadsheet called *SampleSizeDetermination.xlsx*, which is found in the book support files for this chapter. You may want to utilize that spreadsheet as you go through this an subsequent sections. 

Using a simple example, we will illustrate how to determine the sample size necessary to estimate a quantity of interest with a high level of confidence.

***
\BeginKnitrBlock{example}
<span class="example" id="exm:ex1SampleSize"><strong>(\#exm:ex1SampleSize) </strong></span>Suppose that we want to simulate a normally distributed random variable, $X$, with $E[X] = 10$ and $Var[X] = 16$.  From the simulation, we want to estimate the true population mean with 99 percent confidence for a half-width $\pm 0.50$ margin of error.  In addition to estimating the population mean, we want to estimate the probability that the random variable exceeds 8.  That is, we want to estimate, $p= P[X>8]$.  We want to be 95% confident that our estimate of $P[X>8]$ has a margin of error of $\pm 0.05$.  What are the sample size requirements needed to meet the desired margins of error?  
\EndKnitrBlock{example}

***

Using simulation for this example is for illustrative purposes.  Naturally, we actually know the true population mean and we can easily compute the $p= P[X>8]$. for this situation. However, the example will illustrate sample size determination, which is an important planning requirement for simulation experiments.

Let $X$ represent the unknown random variable of interest. Then, we are
interested in estimating $E[X]=\mu$ and $p = P[X > 8]$. To estimate
these quantities, we generate a random sample, $(X_1,X_2,...,X_n)$. $E[X]$ can be
estimated using $\bar{X}$. To estimate a probability it is useful to define an indicator variable. An indicator variable has the value 1 if the condition associated with the probability is true and has the value 0 if it is false. To estimate $p$, define the following indicator
variable:

$$
Y_i = 
   \begin{cases}
     1 & X_i > 8\\
     0 & X_i \leq 8 \\
  \end{cases}
$$
This definition of the indicator variable $Y_i$ allows $\bar{Y}$ to be
used to estimate $p$. Recall the definition of the sample average
$\bar{Y}$. Since $Y_{i}$ is a $1$ only if $X_i > 8$, then the
$\sum \nolimits_{i=1}^{n} Y_{i}$ simply adds up the number of $1$'s in
the sample. Thus, $\sum\nolimits_{i=1}^{n} Y_{i}$ represents the count
of the number of times the event $X_i > 8$ occurred in the sample. Call
this $\#\lbrace X_i > 8\rbrace$. The ratio of the number of times an
event occurred to the total number of possible occurrences represents
the proportion. Thus, an estimator for $p$ is:

$$
\hat{p} = \bar{Y} = \frac{1}{n}\sum_{i=1}^{n}Y_i = \frac{\#\lbrace X_i  > 8\rbrace}{n}
$$
Therefore, computing the average of an indicator variable will estimate
the desired probability. The java-code for this situation is quite simple:

```java
NormalRV rv = new NormalRV(10.0, 4.0);
Statistic estimateX = new Statistic("Estimated X");
Statistic estOfProb = new Statistic("Pr(X>8)");
StatisticReporter r = new StatisticReporter(List.of(estOfProb, estimateX));
int n = 20; // sample size
for (int i = 1; i <= n; i++) {
  double x = rv.getValue();
  estimateX.collect(x);
  estOfProb.collect(x > 8);
}
System.out.println(r.getHalfWidthSummaryReport());
```
In the code, an instance of Statistic is used to observe values of the quantity $(x > 8)$.  This quantity is actually a logical condition which will evaluate to true (1) or false (0) given the value of $x$.  Thus, the Statistic will be recording observations of 1’s and 0’s.  Because the Statistic computes that average of the values that it “collects”, we will get an estimate of the probability.  Thus, the quantity, $(x > 8)$ is an indicator variable for the desired event.  Using a Statistic to estimate a probability in this manner is quite effective.  

Now, in more realistic simulation situations, we would not know the true population mean and variance.  Thus, in solving this problem, we will ignore that information.  To apply the previously discussed sample size determination methods, we need estimates of $\sigma = \sqrt{Var[X]}$  and $p = P[X>8]$.  In order to get estimates of these quantities, we need to run our simulation experiment; however, in order to run our simulation experiment, we need to know how many observations to make.  This is quite a catch-22 situation!  

To proceed, we need to run our simulation model for a pilot experiment.  From a small number of samples (called a pilot experiment, pilot run or pilot study), we will estimate $\sigma$ and $p = P[X>8]$ and then use those estimates to determine how many samples we need to meet the desired margins of error.  We will then re-run the simulation using the recommended sample size.

The natural question to ask is how big should your pilot sample be?  In general, this depends on how much it costs for you to collect the sample.  Within a simulation experiment context, generally, cost translates into how long your simulation model takes to execute.  For this small problem, the execution time is trivial, but for large and complex simulation models the execution time may be in hours or even days.  A general rule of thumb is between 10 and 30 observations. For this example, we will use an initial pilot sample size, $n_0 = 20$. 

Running the model for the 20 replications yields the following results.

Performance Measures    Average   95% Half-Width
--------------------    -------   -------------
  Pr(X>8)               0.70         0.22
  Estimated X           10.3224      2.24

Table: (\#tab:exResults) Simulation results for $n_0 = 20$ replications

Notice that the confidence interval for the true mean is $(8.0824,12.5624)$ with a half-width of $2.24$, which exceeds the desired margin of error, $\epsilon = 0.5$. Notice also that this particular confidence interval happens to contain the true mean $E[X] = 10$. To determine the sample size so that we get a half-width that is less than $\epsilon = 0.1$ with 99% confidence, we can use Equation \@ref(eq:zSampleSize). In order to do this, we must first have an estimate of the sample standard deviation, $s$.  Since Table \@ref(tab:exResults) reports a half-width for a 95% confidence interval, we need to use Equation \@ref(eq:hw) and solve for $s$ in terms of $h$. Rearranging Equation \@ref(eq:hw) yields,

\begin{equation} 
s = \dfrac{h\sqrt{n}}{t_{1-(\alpha/2), n-1}}
  (\#eq:sInTermsOfh)
\end{equation} 

Using the results from Table \@ref(tab:exResults) and $\alpha = 0.05$ in Equation \@ref(eq:sInTermsOfh) yields,

$$
s_0 = \dfrac{h\sqrt{n_0}}{t_{1-(\alpha/2), n_0-1}} = \dfrac{2.24\sqrt{20}}{t_{0.975, 19}}= \dfrac{2.24\sqrt{20}}{2.093}=4.7862
$$

Now, we can use Equation \@ref(eq:zSampleSize) to determine the sample size requirement. For a 99% confidence interval, we have
$\alpha = 0.01$ and $\alpha/2 = 0.005$. Thus, $z_{0.995} = 2.576$. Because the margin of error, $\epsilon$ is $0.5$, we have that,

$$n \geq \left(\dfrac{z_{1-(\alpha/2)}\; s}{\epsilon}\right)^2 = \left(\dfrac{2.576 \times 4.7862}{0.5}\right)^2 = 608.042 \approx 609$$
To determine the sample size for estimating $p=P[X>8]$ with 95% confidence to $\pm 0.1$, we can use Equation \@ref(eq:pSampleSize)

$$
n = \left(\dfrac{z_{1-(\alpha/2)}}{\epsilon}\right)^{2} \hat{p}(1 - \hat{p})=\left(\dfrac{z_{0.975}}{0.1}\right)^{2} (0.22)(1 - 0.22)=\left(\dfrac{1.96}{0.1}\right)^{2} (0.22)(0.78) =65.92 \approx 66
$$
By using the maximum of $\max{(66, 609)=609}$, we can re-run the simulation this number of replications.  Doing so, yields,

Performance Measures    Average   95% Half-Width
--------------------    -------   -------------
   Pr(X>8)               0.69        0.04
   Estimated X           9.9032      0.32

Table: (\#tab:exResultsn609) Simulation Results for $n=609$ Replications

As can be seen in Table \@ref(tab:exResultsn609), the half-width values meet the desire margins of error. It may be possible that the margins of error might not be met. This suggests that more than $n = 609$ observations is needed to meet the margin of error criteria. Equation \@ref(eq:zSampleSize) and Equation \@ref(eq:pSampleSize) are only approximations and based on a pilot sample. Thus, if there was considerable sampling error associated
with the pilot sample, the approximations may be inadequate.

As can be noted from this example in order to apply the normal approximation method for determining the sample size based on the pilot run, we need to compute the initial sample standard deviation, $s_0$, from the initial reported half-width, $h$. This requires the use of Equation \@ref(eq:sInTermsOfh) to first compute the value of $s$ from $h$.  We can avoid this calculation by using the *half-width ratio* method for determining the sample size. 

Let $h_0$ be the initial value for the half-width from the pilot run of
$n_0$ replications. Then, rewriting Equation \@ref(eq:hw) in terms of the pilot data yields:

$$
h_0 = t_{1-(\alpha/2), n_0 - 1} \dfrac{s_0}{\sqrt{n_0}}
$$
Solving for $n_0$ yields:

\begin{equation} 
n_0 = t_{1-(\alpha/2), n_0 -1}^{2} \dfrac{s_{0}^{2}}{h_{0}^{2}}
  (\#eq:nNottsh)
\end{equation} 

Similarly for any $n$, we have:

\begin{equation} 
n = t_{1-(\alpha/2), n-1}^{2} \dfrac{s^{2}}{h^{2}}
  (\#eq:ntsh)
\end{equation} 

Taking the ratio of $n_0$ to $n$ (Equations \@ref(eq:nNottsh) and \@ref(eq:ntsh)) and assuming
that $t_{1-(\alpha/2), n-1}$ is approximately equal to
$t_{1-(\alpha/2), n_0 - 1}$ and $s^2$ is approximately equal to $s_0^2$,
yields,

$$n \cong n_0 \dfrac{h_0^2}{h^2} = n_0 \left(\frac{h_0}{h}\right)^2$$

This is the half-width ratio equation.

\begin{equation} 
n \geq n_0 \left(\frac{h_0}{h}\right)^2
  (\#eq:hwratio)
\end{equation} 

The issue that we face when applying Equation \@ref(eq:hwratio) is that Table \@ref(tab:exResults) only reports the 
95\% confidence interval half-width.  Thus, to apply Equation \@ref(eq:hwratio) the value of $h_0$ will be based on an $\alpha = 0.05$.  If the desired half-width, $h$, is required for a different confidence level, e.g. a 99\% confidence interval, then you must first translate $h_0$ to be based on the desired confidence level or set the confidence level for the StatisticalReporter to be 99\%. To compute the 95\% half-width from Table \@ref(tab:exResults), you can compute $s$ from Equation \@ref(eq:sInTermsOfh) and then recomputing the actual half-width, $h$, for the desired confidence level.   

Using the results from Table \@ref(tab:exResults) and $\alpha = 0.05$ in Equation \@ref(eq:sInTermsOfh) we already know that $s_0 = 4.7862$ for a 95\% confidence interval. Thus, for a 99\% confidence interval, where $\alpha = 0.01$ and $\alpha/2 = 0.005$, we have:

$$
h_0 = t_{1-(\alpha/2), n_0-1} \dfrac{s_0}{\sqrt{n_0}}= t_{0.995, 19}\dfrac{4.7862}{\sqrt{20}}=2.861\dfrac{4.7862}{\sqrt{20}}=3.0618
$$
Now, we can apply Equation \@ref(eq:hwratio) to this problem with the desire half-width bound $\epsilon = 0.5 = h$:

$$
n \geq n_0 \left(\frac{h_0}{h}\right)^2=20\left(\frac{3.0618}{h}\right)^2=20\left(\frac{3.0618}{0.5}\right)^2=749.98 \cong 750
$$
We see that for this pilot sample, the half-width ratio method recommends a substantially large sample size, $750$ versus $609$. In practice, the half-width ratio method tends to be conservative by recommending a larger sample size. We will see another example of these methods within the next section.

Based on these examples, you should have a basic understanding of how a
simulation experiment can be performed to meet a desired margin of error. In general, simulation models are much more interesting than this simple example. 

## Simulating the Game of Craps {#ch5-craps}

Consider the game of “craps” as played in Las Vegas.  The basic rules of the game are as follows:  one player, the “shooter”, rolls a pair of dice.  If the outcome of that roll is a 2, 3, or 12, the shooter immediately loses; if it is a 7 or an 11, the shooter wins.  In all other cases, the number the shooter rolls on the first toss becomes the “point”, which the shooter must try to duplicate on subsequent rolls.  If the shooter manages to roll the point before rolling a 7, the shooter wins; otherwise the shooter loses.  It may take several rolls to determine whether the shooter wins or loses.  After the first roll, only a 7 or the point have any significance until the win or loss is decided.  Using the JSL random and statistic packages give answers and corresponding estimates to the following questions. Be sure to report your estimates in the form of confidence intervals.

a)	Before the first roll of the dice, what is the probability the shooter will ultimately win?
b)	What is the expected number of rolls required to decide the win or loss?
 
The solution to this problem involves the use of the Statistic class to collect the probability that the shooter will win and for the expected number of rolls.  The following code illustrates the approach.  The `DUniformRV` class is used to represent the two dice.  Two statistics are created to estimate the probability of winning and to estimate the expected number of rolls required to decide a win or a loss.  A for loop is use to simulate 5000 games.  For each game, the logic of winning, losing or matching the first point.  When the game is ended the instances of `Statistic` are used to collect the outcomes.

```java
DUniformRV d1 = new DUniformRV(1, 6);
DUniformRV d2 = new DUniformRV(1, 6);
Statistic probOfWinning = new Statistic("Prob of winning");
Statistic numTosses = new Statistic("Number of Toss Statistics");
int numGames = 5000;
for (int k = 1; k <= numGames; k++) {
	boolean winner = false;
	int point = (int) d1.getValue() + (int) d2.getValue();
	int numberoftoss = 1;

	if (point == 7 || point == 11) {
		// automatic winner
		winner = true;
	} else if (point == 2 || point == 3 || point == 12) {
		// automatic loser
		winner = false;
	} else { // now must roll to get point
		boolean continueRolling = true;
		while (continueRolling == true) {
			// increment number of tosses
			numberoftoss++; 
			// make next roll
			int nextRoll = (int) d1.getValue() + (int) d2.getValue();
			if (nextRoll == point) {
				// hit the point, stop rolling
				winner = true;
				continueRolling = false;
			} else if (nextRoll == 7) {
				// crapped out, stop rolling
				winner = false;
				continueRolling = false;
			}
		}
	}
	probOfWinning.collect(winner);
	numTosses.collect(numberoftoss);
}
StatisticReporter reporter = new StatisticReporter();
reporter.addStatistic(probOfWinning);
reporter.addStatistic(numTosses);
System.out.println(reporter.getHalfWidthSummaryReport());
```
As we can see from the results of the statistics reporter, the probability of winning is just a little less than 50 percent.

```
Half-Width Statistical Summary Report - Confidence Level (95.000)% 

Name                            Count 	      Average 	   Half-Width 
-------------------------------------------------------------------------------- 
Prob of winning                   5000 	       0.4960 	       0.0139 
Number of Toss Statistics         5000 	       3.4342 	       0.0856 
-------------------------------------------------------------------------------- 
```

Now let's look at a slightly more complex static simulation.

\FloatBarrier

## The News Vendor Problem {#ch5-newsvendor}

The news vendor model is a classic inventory model that allows for the
modeling of how much to order for a decision maker facing uncertain
demand for an upcoming period of time. It takes on the news vendor
moniker because of the context of selling newspapers at a newsstand. The
vendor must anticipate how many papers to have on hand at the beginning
of a day so as to not run short or have papers left over. This idea can
be generalized to other more interesting situations (e.g. air plane
seats). Discussion of the news vendor model is often presented in
elementary textbooks on inventory theory. The reader is referred to
[@Muckstadt2010aa] for a discussion of this model.

The basic model is considered a single period model; however, it can be
extended to consider an analysis covering multiple (or infinite) future
periods of demand. The representation of the costs within the modeling
offers a number of variations.

Let $D$ be a random variable representing the demand for the period. Let
$F(d)= P[D \leq d]$ be the cumulative distribution function for the
demand. Define $G(q,D)$ as the profit at the end of the period when $q$
units are ordered at the start of the period with $D$ units of demand.
The parameter $q$ is the decision variable associated with this model.
Depending on the value of $q$ chosen by the news vendor, a profit or
loss will occur.

There are two cases to consider $D \geq q$ or $D < q$. That is, when
demand is greater than or equal to the amount ordered at the start of
the period and when demand is less than the amount ordered at the start
of the period. If $D \geq q$ the news vendor runs out of items, and if
$D < q$ the news vendor will have items left over.

In order to develop a profit model for this situation, we need to define
some model parameters. In addition, we need to determine the amount that
we might be short and the amount we might have left over in order to
determine the revenue or loss associated with a choice of $q$. Let $c$
be the purchase cost. This is the cost that the news vendor pays the
supplier for the item. Let $s$ be the selling price. This is the amount
that the news vendor charges customers. Let $u$ be the salvage value
(i.e. the amount per unit that can be received for the item after the
planning period). For example, the salvage value can be the amount that
the news vendor gets when selling a left over paper to a recycling
center. We assume that selling price $>$ purchase cost $>$ salvage
value.

Consider the context of a news vendor planning for the day. What is the
possible amount sold? If $D$ is the demand for the day and we start with
$q$ items, then the most that can be sold is $\min(D, q)$. That is, if
$D$ is bigger than $q$, you can only sell $q$. If $D$ is smaller than
$q$, you can only sell $D$.

What is the possible amount left over (available for salvage)? If $D$ is
the demand and we start with $q$, then there are two cases: 1) demand is
less than the amount ordered ($D < q$) or 2) demand is greater than or
equal to the amount ordered ($D \geq q$). In case 1, we will have $q-D$
items left over. In case 2, we will have $0$ left over. Thus, the amount
left over at the end of the day is $\max(0, q-D)$. Since the news vendor
buys $q$ items for $c$, the cost of the items for the day is
$c \times q$. Thus, the profit has the following form:

\begin{equation}
G(D,q) = s \times \min(D, q) + u \times \max(0, q-D) - c \times q
(\#eq:ExpRev)
\end{equation}

In words, the profit is equal to the sales revenue plus the salvage
revenue minus the ordering cost. The sales revenue is the selling price
times the amount sold. The salvage revenue is the salvage value times
the amount left over. The ordering cost is the cost of the items times
the amount ordered. To find the optimal value of $q$, we should try to
maximize the expected profit. Since $D$ is a random variable, $G(D,q)$
is also a random variable. Taking the expected value of both sides of
Equation \@ref(eq:ExpRev), yields:

$$g(q) = E[G(D,q)] =  sE[\min(D, q)] + uE[\max(0, q-D)] - cq$$

Whether or not this equation can be optimized depends on the form of the
distribution of $D$; however, simulation can be used to estimate this
expected value for any given $q$. Let's look at an example.

***

\BeginKnitrBlock{example}
<span class="example" id="exm:BBQWing"><strong>(\#exm:BBQWing) </strong></span>Sly's convenience store sells BBQ wings for 25 cents a piece. They cost 15
cents a piece to make. The BBQ wings that are not sold on a given day
are purchased by a local food pantry for 2 cents each. Assuming that Sly
decides to make 30 wings a day, what is the expected revenue for the
wings provided that the demand distribution is as show in
Table \@ref(tab:BBQWingDemand).

\EndKnitrBlock{example}

***

::: {#tab:BBQWingDemand}
    $d_{i}$      5    10    40    45    50     55     60
  ------------ ----- ----- ----- ----- ----- ------ ------
   $f(d_{i})$   0.1   0.2   0.3   0.2   0.1   0.05   0.05
   $F(d_{i})$   0.1   0.3   0.6   0.8   0.9   0.95   1.0

  Table: (\#tab:BBQWingDemand) Distribution of BBQ wing demand
:::

The code for the news vendor
problem will require the generation of the demand from BBQ Wing demand distribution. In order to generate from this distribution, the DEmpirical class should be used. Since the cumulative distribution has already been computed, it
is straight forward to write the inputs for the DEmpirical class. 

```java
double q = 30; // order qty
double s = 0.25; //sales price
double c = 0.15; // unit cost
double u = 0.02; //salvage value
double[] values = {5, 10, 40, 45, 50, 55, 60};
double[] cdf = {0.1, 0.3, 0.6, 0.8, 0.9, 0.95, 1.0};
DEmpiricalRV dCDF = new DEmpiricalRV(values, cdf);
Statistic stat = new Statistic("Profit");
double n = 100; // sample size
for (int i = 1; i <= n; i++) {
    double d = dCDF.getValue();
    double amtSold = Math.min(d, q);
    double amtLeft = Math.max(0, q - d);
    double g = s * amtSold + u * amtLeft - c * q;
    stat.collect(g);
}
System.out.printf("%s \t %f %n", "Count = ", stat.getCount());
System.out.printf("%s \t %f %n", "Average = ", stat.getAverage());
System.out.printf("%s \t %f %n", "Std. Dev. = ", stat.getStandardDeviation());
System.out.printf("%s \t %f %n", "Half-width = ", stat.getHalfWidth());
System.out.println(stat.getConfidenceLevel() * 100 + "% CI = " + stat.getConfidenceInterval());
```
Running the model for 100 days results in the following output.

```
Count =  	 100.000000 
Average =  	 1.677500 
Std. Dev. =  	 2.201324 
Half-width =  	 0.436790 
95.0% CI = [1.2407095787617952, 2.1142904212382048]
```

## Summary

In this chapter, we explored how to develop models in for which time is
not a significant factor. In the case of the news vendor problem, where
we simulated each day's demand, time advanced at regular intervals. In
the case of the area estimation problem, time was not a factor in the simulation.
These types of simulation experiments are often termed static. In the
next chapter, we begin our exploration of simulation experiments where time is an
integral component in driving the behavior of the system. In addition,
we will see that time will not necessarily advance at regular intervals
(e.g. hour 1, hour 2, etc.). This will be the focus of the rest of the
book.
