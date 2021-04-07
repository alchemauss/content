---
title: Calculus II - Complete Cumulative Notes
date:updated: 2021-03-25
---

> Calculus of Logarithmic and Exponential Function, Techniques for Solving Integrals, Series, and Calculus with Polar Coordinates.

![!YouTube-S#hb](PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr "Grant Sanderson - The Essence of Calculus")

<!-- ![!YouTube](CfW845LNObM "What they won't teach you in calculus") -->

## Preliminary - Introduction & Reviews

### Tangent Line

The $\text{tangent line (1)}$ to the function $f(x)$ at the point $x=a$ is a line that touches the graph of the function only at the point in question as is parallel (in some way) to the graph at that point, provided that the limit exists and is not $\infin$ or $-\infin$.

$$\tag{1}
m_{tan} = \lim_{h \to 0}\ m_{sec} = \lim_{h \to 0}\ \frac{f(c+h)-f(c)}{h}
\\[0.75em] \text{or for average velocity} \\[0.75em]
v = \lim_{h \to 0}\ v_{avg} = \lim_{h \to 0}\ \frac{f(c+h)-f(c)}{h}
$$

***

$\bold{Example.}$ Find the tangent line to $f(x)=15-2x^2$ at $x=1$.

1. The tangent line and the graph of the function must touch at $x=1$ so the point $(1,f(1))=(1,13)$ must be on the line, we can say this point as $P$.
2. Pick another point that lies on the graph of the function, let's call that $Q=(x,f(x))$. Let's choose $x=2$ so the second point will be $Q=(2,7)$.
3. As an estimate of the slope of the tangent line, we can use the slope of the secant line, which is $m_{PQ}=\frac{f(2)-f(1)}{2-1}=\frac{7-13}{1}=-6$. This is good enough if we weren't too interested in accuracy, but we can get a better estimate with $x$ closer to $x=1$.
4. Converting the previous formula into a more general form of $m_{PQ}=\frac{f(x)-f(1)}{x-1}=\frac{15-2x^2-13}{x-1}=\frac{2-2x^2}{x-1}$, we can then pick some values closer to $x=1$ and see the slope of the secant lines appears to be approaching -4.
5. The equation of the line that goes through $(a,f(a))$ is given by $y=f(a)+m(x-a)$. Therefore, the equation of the tangent line is $y=13-4(x-1)=-4x+17$

### Rates of Change

Consider a function $f(x)$ that represents some quantity that varies as $x$ varies. We want to determine just how fast $f(x)$ is changing at some point, say $x=a$. This is called the **instantaneous rate of change** or sometimes just **rate of change** of $f(x)$ at $x=a$. To compute tha average rate of change of $f(x)$ at $x=a$, we will choose another point, say $x$ and the average rate of change will be

$$
\begin{aligned}
  A.R.C. &= \frac{\text{change in }f(x)}{\text{change in }x}
  \\[1em] &= \frac{f(x)-f(a)}{x-a}
\end{aligned}
$$

The same applies from finding the tangent line, we can estimate the instantaneous rate of change at $x=a$ by choosing the values of $x$ and getting closer to $x=a$ (don't forget to do it from both sides) and compute the values of $A.R.C.$

***

$\bold{Example.}$ Suppose that the amount of air in a balloon after $t$ hours is given by $V(t)=t^3-6^2+35.$ Estimate the instantaneous rate of change of the volume after 5 hours.

1. Get the formula for the average rate of change of the volume, in this case $A.R.C.=\frac{V(t)-V(5)}{t-5}=\frac{t^3-6t^2+35-10}{t-5}=\frac{t^3-6t^2+25}{t-5}$
2. Pick values of $t$ that are getting closer to $t=5$, display it in a table and you can see that the average rate of change is approaching 15 and so we can estimate that the instantaneous rate of change is 15 at this point.

### Derivative

Differentiation takes a function $f$ and produces a new function $f'$ (f prime) whose value at any number x is defined as

$$
f'(x) = \lim_{h \to 0}\frac{f(x+h)-f(x)}{h}
\\[0.75em] \text{or an equivalent form} \\[0.75em]
f'(c) = \lim_{x \to c}\frac{f(x)-f(c)}{x-c}
$$

If the limit does exist, we say that $f$ is $\text{\color{deeppink}differentiable}$ at $x$. Finding a derivative is called $\text{\color{deeppink}differentiation}$. If $f'(c)$ exists, then $f$ is continuous at $c$. The Leibniz notation for the derivative is

$$
\begin{aligned}
  \frac{dy}{dx} &= \lim_{\Delta{x} \to 0}\frac{\Delta{y}}{\Delta{x}} \\[1em]
  &= \lim_{\Delta{x} \to 0}\frac{f(x+\Delta{x})-f(x)}{\Delta{x}} \\[1em]
  &= f'(x)
\end{aligned}
$$

***

$\text{\color{aqua}\large Constant and Power Rules}$

$\text{Theorem A}\quad\lfloor\text{Constant Function Rule}\rceil$

If $f(x)=k$, where $k$ is a constant, then for any $x, f'(x)=0$; that is $D_x(k)=0$. For example, $$\begin{aligned}f(x) &= 1000 \\ f'(x) &= 0\end{aligned}$$

$\text{Theorem B}\quad\lfloor\text{Identity Function Rule}\rceil$

If $f(x)=x$, then $f'(x)=1$; that is, $D_x(x)=1$. For example, $$\begin{aligned}f(x) &= x \\ f'(x) &= 1\end{aligned}$$

$\text{Theorem C}\quad\lfloor\text{Power Rule}\rceil$

If $f(x)=x^n$, where $n$ is a positive integer, then $f'(x)=nx^{n-1}$; that is, $D_x(x^n)=nx^{n-1}$. For example, $$\begin{aligned}f(x) &= x^3 \\ f'(x) &= 3x^2\end{aligned}$$

$\text{Theorem D}\quad\lfloor\text{Constant Multiple Rule}\rceil$

If $k$ is a constant and $f$ is a differentiable function, then $(kf)'(x)=k \sdot f'(x)$; that is, $D_x\lbrack k \sdot f(x)\rbrack=k \sdot D_x f(x)$. Meaning, a constant multiplier $k$ can be passed across the operator $D_x$.

$\text{Theorem E}\quad\lfloor\text{Sum Rule}\rceil$

If $f$ and $g$ are differentiable functions, then $(f+g)'(x)=f'(x)+g'(x)$; that is, $D_x\lbrack f(x)+g(x)\rbrack=D_x f(x)+D_x g(x)$. Meaning, the derivative of a sum is the sum of the derivatives.

$\text{Theorem F}\quad\lfloor\text{Difference Rule}\rceil$

If $f$ and $g$ are differentiable functions, then $(f-g)'(x)=f'(x)-g'(x)$; that is, $D_x\lbrack f(x)-g(x)\rbrack=D_x f(x)-D_x g(x)$.

$\text{Theorem G}\quad\lfloor\text{Product Rule}\rceil$

If $f$ and $g$ are differentiable functions, then $(f\sdot g)'(x)=f(x)g'(x)+g(x)f'(x)$; In other words, $$u'v+v'u$$

$\text{Theorem H}\quad\lfloor\text{Quotient Rule}\rceil$

Let $f$ and $g$ be differentiable functions with $g(x)!=0$. Then $(\frac{f}{g})'(x)=\frac{g(x)f'(x)-f(x)g'(x)}{g^2(x)}$; In other words, $$\frac{u'v-v'u}{v^2}$$

$\text{\color{aqua}\large Derivative Formulas}$

$\text{Theorem A}$

The functions $f(x)=\sin x$ and $g(x)=\cos x$ are both differentiable, $$D_c$$

***

### Techniques of Integration

### Higher-Order Derivatives

## Improper Integrals

Integral by definition is $\int_a^b f(x) dx$ where $f(x)$ must be **bounded** & **finite** on $[a,b]$. When it doesn't satisfy at least one of the constraint, it's considered an improper integral.

![!YouTube#d#hb](g-M8FHslgdk "Professor Leonard - Lecture 7.6: Improper Integrals")

### Indeterminate Forms

Consists of only 7 (deadly sins) forms which are as follows

$$
\bigg[
  \frac{0}{0},
  \ \frac{\infin}{\infin},
  \ 0 \sdot \infin,
  \ \infin - \infin,
  \ 0^0,
  \ \infin^0,
  \ 1^\infin
\bigg]
$$

Keep in mind that these forms are written in the context of limits and functions, so the $0$s and $1$ we see here are the value of $x$ in $f(x)$ as $x$ approaches that number, hence why $1^\infin$ is considered an indeterminate form.

An improper integral result can be categorized into two category, which is **convergent** when the value of the limit exists, and **divergent** when the value of the limit doesn't exists (D.N.E.).

### One Finite Limit

$$
\int_{-\infin}^{-1}
$$

### Both Infinite

Evaluate $\int_{-\infin}^\infin \frac{1}{1+x^2}\ dx$ or state that it diverges

$$
\begin{aligned}
  \int
\end{aligned}
$$

***

> L'Hospital can only be applied to the indeterminate form of $\frac{0}{0}$ or $\frac{\infin}{\infin}$.

$$
\begin{aligned}
  \log x &\not = \ln x \\
  \log_{10} x &\not = \log_e x
\end{aligned}
$$

$$
\begin{aligned}
  y &= e^{\ln y} \\
  a^x \to \ln a^x &= x \ln a \\
  a \to e^{\ln a} &= a
\end{aligned}
$$

## Series & Sequences

$$
\begin{aligned}
  \text{Sequence} &: 1,2,3,4
  \text{Series} &: 1+2+3+4
\end{aligned}
$$

Review:

- Teorema2 Limit
- Teorema Limit Utama
- Teorema Substitusi
- Teorema Apit

> The convergence or divergence will not change even when a finite number of terms is added or subtracted, but the sum will change

A

### Properties of Limit of Sequences

### Alternating Sequence

$\text{Theorem C}$ If $\lim_{n \to \infin}|a_n| = 0$, then $\lim_{n \to \infin}a_n = 0$

$\bold{Example.}$ Does the sequence $a_n = (-1)^n \frac{n}{n^2+2}$ converge or diverge.

## Parametric Equations & Polar Coordinates

$$\cot{2\theta} = \frac{a-c}{b}$$

### Parametric Equations

![!YouTube#d#hb](d4KADBFqpR0 "Professor Leonard - Lecture 10.2: Introduction to Parametric Equations")

### Calculus of Parametric Equations

![!YouTube#d#hb](1H6HrfX_qCA "Professor Leonard - Lecture 10.3: Calculus of Parametric Equations")

### Polar Coordinates

![!YouTube#d#hb](sWUyFQQ5QeI "Professor Leonard - Lecture 10.4: Using Polar Coordinates and Polar Equations")

![polar graph#f](https://i.stack.imgur.com/35dGX.jpg "Source: [Math StackExchange](https://math.stackexchange.com/a/771678)")

$$P(r,\theta)$$

#### Polar to Rectangular Coordinates

$$
\begin{aligned}
  x &= r \cos{\theta}
  y &= r \sin{\theta}
\end{aligned}
$$

Don't get those two confused, $x$ is the $\cos$ and $y$ is the $\sin$.

#### Rectangular to Polar Coordinates

We need our $r$ and $\theta$, we'll start by finding our $r$ using this trigonometry identity $r^2 = x^2 + y^2$. Next, we'll find our $\theta$ using $\tan{\theta} = \frac{y}{x}$. That will result a $\tan$ value that we can map into the polar graph.

#### Symmetry Graphing of Polar Graph

#### Finding Tangent Lines of Polar Graph

The slope can be calculated by finding the derivative using the following equation

$$
\frac{dy}{dx} = \frac
  {\frac{dr}{d\theta}\sin\theta + r\cos\theta}
  {\frac{dr}{d\theta}\cos\theta - r\sin\theta}
$$

Plug in the $\theta$ to the final equation and that's your gradient slope right there ($m$).

### Calculus of Polar Equations

![!YouTube#d#hb](Kh265EC11OI "Professor Leonard - Lecture 10.5: Calculus of Polar Equations")

#### Area Bound by Polar Curve

#### Area Between Two Polar Curves

## Extras - Online Tools & References

![!YouTube-S#hb](PLDesaqWTN6EQ2J4vgsN1HyBeRADEh4Cw- "Professor Leonard - Calculus 2 Lectures")
