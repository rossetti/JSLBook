# Monte Carlo Methods {#mcm}

**[Learning Objectives]{.smallcaps}**

-   To be able to use the JSL to perform basic Monte Carlo simulation
-	  To illustrate generating and collecting statistics for Monte Carlo simulation

This chapter illustrates how to use the JSL for simple Monte-Carlo
simulations. The term Monte Carlo generally refers to the set of methods
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

## Simulating the Game of Craps {#craps}

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

Name                                     	        Count 	      Average 	   Half-Width 
---------------------------------------------------------------------------------------------------- 
Prob of winning                          	         5000 	       0.4960 	       0.0139 
Number of Toss Statistics                	         5000 	       3.4342 	       0.0856 
---------------------------------------------------------------------------------------------------- 
```
 
 
