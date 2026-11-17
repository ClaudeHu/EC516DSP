# EC 516 Midterm 2 Review (Problem Sets 1–8)

Built from the posted solutions to PS01–PS08 and the Fall 2026 Midterm-2 formula sheet. All math is written for GitHub's math renderer, so it displays correctly on a GitHub repo page.

**How to use this note.** Sections 1–2 are the cheat-sheet layer: what the formula sheet gives you and what it does not. Sections 3–11 follow the problem sets topic by topic with the worked solutions condensed. Section 13 is a one-glance answer key. Sections 14–15 are for the last pass before the exam.

## Contents

- [Topic map](#topic-map)
- [1. Formula sheet at a glance](#1-formula-sheet-at-a-glance)
- [2. Not on the formula sheet: memorize these](#2-not-on-the-formula-sheet-memorize-these)
- [3. Sequences, convolution, and LTI basics (PS1)](#3-sequences-convolution-and-lti-basics-ps1)
- [4. Sinusoids through LTI systems (PS1, PS2, PS3)](#4-sinusoids-through-lti-systems-ps1-ps2-ps3)
- [5. DTFT calculations and properties (PS2, PS4)](#5-dtft-calculations-and-properties-ps2-ps4)
- [6. z-transform, ROC, and inverse z-transform (PS3, PS4)](#6-z-transform-roc-and-inverse-z-transform-ps3-ps4)
- [7. Pole-zero geometry and frequency shifting (PS5)](#7-pole-zero-geometry-and-frequency-shifting-ps5)
- [8. Group delay and phase delay (PS4, PS6, PS7)](#8-group-delay-and-phase-delay-ps4-ps6-ps7)
- [9. Generalized linear-phase FIR filters (PS5, PS6)](#9-generalized-linear-phase-fir-filters-ps5-ps6)
- [10. Minimax FIR design with the alternation theorem (PS7)](#10-minimax-fir-design-with-the-alternation-theorem-ps7)
- [11. DFT, frequency sampling, and time aliasing (PS8)](#11-dft-frequency-sampling-and-time-aliasing-ps8)
- [12. On the sheet but not in PS 1-8](#12-on-the-sheet-but-not-in-ps-1-8)
- [13. Answer key by problem](#13-answer-key-by-problem)
- [14. Exam traps checklist](#14-exam-traps-checklist)
- [15. Rapid self-check](#15-rapid-self-check)

## Topic map

| PS | Problems | Topics |
|---|---|---|
| 1 | 2.21, 2.29, 2.30, 2.62 | index manipulation, step response, cascades, sinusoid through a complex system |
| 2 | 2.32, 2.37, 2.41, 2.45 | real/imaginary/magnitude/phase of a DTFT, ideal filters in cascade, reading phase plots, mixed inputs |
| 3 | 3.21, 3.30, 3.37, 3.40 | ROC, stability, partial fractions, left-sided inputs, time reversal, finding $`H(z)`$ from $`x`$ and $`y`$ |
| 4 | 2.55, 3.44, 5.63 | DTFT shortcuts, initial values from a difference equation, group delay vs phase delay |
| 5 | 5.25, 5.28, 5.37, 5.58 | modulation by $`(-1)^n`$, notch filters, linear-phase design from constraints, type mapping |
| 6 | 5.19, 5.46, 5.57 | group delay from symmetry, cascaded linear-phase filters, the four amplitude forms |
| 7 | minimax design | alternation theorem, Chebyshev substitution, optimal 3-tap lowpass |
| 8 | 8.6, 8.7, 8.8, 8.10, 8.32 | DFT of a truncated exponential, frequency sampling and time aliasing, circular shift |

---

## 1. Formula sheet at a glance

Condensed from the official sheet, with the sheet's notation kept so you can find things fast during the exam.

**Basic signals and Euler**

$$
u[n]=\begin{cases}1 & n\ge 0\\ 0 & \text{otherwise}\end{cases}\qquad\qquad \delta[n]=\begin{cases}1 & n=0\\ 0 & \text{otherwise}\end{cases}
$$

$$
e^{j\omega n}=\cos(\omega n)+j\sin(\omega n)\qquad \cos(\omega n)=\frac{1}{2}\left(e^{j\omega n}+e^{-j\omega n}\right)\qquad \sin(\omega n)=\frac{1}{2j}\left(e^{j\omega n}-e^{-j\omega n}\right)
$$

**DTFT, convolution, geometric sums**

$$
X(e^{j\omega})=\sum_{n=-\infty}^{\infty}x[n]e^{-j\omega n}\qquad\qquad x[n]=\frac{1}{2\pi}\int_{-\pi}^{\pi}X(e^{j\omega})e^{j\omega n}d\omega
$$

$$
y[n]=\sum_{k=-\infty}^{\infty}x[k]h[n-k]
$$

$$
\sum_{k=0}^{N-1}\alpha^k=\begin{cases}\dfrac{1-\alpha^N}{1-\alpha} & \alpha\ne 1\\ N & \alpha=1\end{cases}\qquad\qquad \sum_{k=0}^{\infty}\alpha^k=\frac{1}{1-\alpha}\quad\text{for } |\alpha|<1
$$

**DTFT properties and pairs**

| Sequence | DTFT |
|---|---|
| $`x[n-n_0]`$ | $`e^{-j\omega n_0}X(e^{j\omega})`$ |
| $`e^{j\omega_0 n}x[n]`$ | $`X(e^{j(\omega-\omega_0)})`$ |
| $`x^*[n]`$ | $`X^*(e^{-j\omega})`$ |
| $`x[-n]`$ | $`X(e^{-j\omega})`$ |
| $`x[n]*h[n]`$ | $`X(e^{j\omega})H(e^{j\omega})`$ |
| $`e^{j\omega_0 n}`$ | $`2\pi\sum_{k=-\infty}^{\infty}\delta(\omega-\omega_0-2\pi k)`$ |
| $`\delta[n-n_0]`$ | $`e^{-j\omega n_0}`$ |
| $`u[n]-u[n-N]`$ | $`\dfrac{\sin(\omega N/2)}{\sin(\omega/2)}e^{-j\omega(N-1)/2}`$ |
| $`\dfrac{\sin(\omega_0 n)}{\pi n}`$ | $`1`$ for $`0\le\lvert\omega\rvert\le\omega_0`$ and $`0`$ for $`\omega_0<\lvert\omega\rvert\le\pi`$ |

**z-transform**

| Sequence | z-transform |
|---|---|
| definition | $`X(z)=\sum_{n=-\infty}^{\infty}x[n]z^{-n}`$ |
| $`x[n-n_0]`$ | $`z^{-n_0}X(z)`$ |
| $`x[-n]`$ | $`X(z^{-1})`$ |
| $`x^*[n]`$ | $`X^*(z^*)`$ |
| $`x[n]*h[n]`$ | $`X(z)H(z)`$ |
| $`a^n u[n]`$ | $`\dfrac{1}{1-az^{-1}}`$ with ROC $`\lvert z\rvert>\lvert a\rvert`$ |

**Difference equations (watch the sign convention)**

$$
\text{recursive: } y[n]=\sum_{k=1}^{N}a_k y[n-k]+\sum_{m=0}^{M}b_m x[n-m]\qquad\qquad \text{non-recursive: } y[n]=\sum_{k=0}^{M}b_k x[n-k]
$$

> [!NOTE]
> The sheet puts the feedback terms on the right with a plus sign, so $`H(z)=\dfrac{\sum_m b_m z^{-m}}{1-\sum_k a_k z^{-k}}`$. The PS solutions write equations like $`y[n]+0.25y[n-1]-0.125y[n-2]=\dots`$, which in the sheet's notation means $`a_1=-0.25`$ and $`a_2=0.125`$.

**Filter classes and transformations**

- **FIR:** rational $`H(z)`$ and $`h[n]=0`$ for $`n<0`$ and $`n>N-1`$ (length $`N`$).
- **IIR:** $`h[n]=0`$ for $`n<0`$, and $`H(z)`$ is rational with at least one pole not at the origin.
- **Generalized linear phase:** Type I odd length symmetric, Type II even length symmetric, Type III odd length antisymmetric, Type IV even length antisymmetric.
- **Bilinear transformation:** $`H_d(z)=H_a\left(\dfrac{1-z^{-1}}{1+z^{-1}}\right)`$.

**Alternation theorem and Chebyshev polynomials**

> $`P(x)`$ is the unique $`L`$-th order polynomial that minimizes $`\lVert E\rVert`$ if and only if $`E(x)`$ exhibits at least $`L+2`$ alternations: points $`x_1<x_2<\dots<x_{L+2}`$ in $`I_C`$ with $`E(x_i)=-E(x_{i+1})`$ and $`|E(x_i)|=\lVert E\rVert`$.

$$
\cos(k\omega)=\Gamma_k(\cos\omega)\qquad \Gamma_0(x)=1\qquad \Gamma_1(x)=x\qquad \Gamma_n(x)=2x\Gamma_{n-1}(x)-\Gamma_{n-2}(x)
$$

From the recursion: $`\Gamma_2(x)=2x^2-1`$, $`\Gamma_3(x)=4x^3-3x`$, $`\Gamma_4(x)=8x^4-8x^2+1`$.

**DFT and downsampling**

$$
X[k]_N=\sum_{n=0}^{N-1}x[n]e^{-j2\pi kn/N}\quad 0\le k\le N-1\qquad\qquad x[n]=\frac{1}{N}\sum_{k=0}^{N-1}X[k]_N e^{j2\pi kn/N}\quad 0\le n\le N-1
$$

$$
y[n]=x[nM]\quad\Longrightarrow\quad Y(e^{j\omega})=\frac{1}{M}\sum_{k=0}^{M-1}X\left(e^{j\left(\frac{\omega}{M}-\frac{2\pi k}{M}\right)}\right)
$$

---

## 2. Not on the formula sheet: memorize these

| Fact | Formula | Used in |
|---|---|---|
| Left-sided exponential | $`-a^n u[-n-1]\longleftrightarrow\dfrac{1}{1-az^{-1}}`$ with ROC $`\lvert z\rvert<\lvert a\rvert`$ | 3.21 (e, f) |
| ROC and causality | causal means ROC is $`\lvert z\rvert>r_{\max}`$ **and includes** $`z=\infty`$ | 3.21, 3.40, 5.28 |
| ROC and stability | stable means ROC contains $`\lvert z\rvert=1`$; causal and stable means all poles inside the unit circle | 3.21, 3.30, 3.40 |
| Exponential weighting | $`a^n x[n]\longleftrightarrow X(z/a)`$, so $`(-1)^n x[n]\longleftrightarrow X(-z)`$ and $`X(e^{j(\omega-\pi)})`$ | 5.25, 5.58 |
| Damped cosine | $`r^n\cos(\omega_0 n)u[n]\longleftrightarrow\dfrac{1-r\cos\omega_0 z^{-1}}{1-2r\cos\omega_0 z^{-1}+r^2z^{-2}}`$ | 3.44 (c) |
| Conjugate pair product | $`(1-re^{j\theta}z^{-1})(1-re^{-j\theta}z^{-1})=1-2r\cos\theta z^{-1}+r^2z^{-2}`$ | 5.28 |
| Real-sequence symmetry | $`X(e^{-j\omega})=X^*(e^{j\omega})`$: magnitude and real part even, phase and imaginary part odd | 2.32, 2.41 |
| Even part | $`\mathrm{Re}\{X(e^{j\omega})\}\longleftrightarrow\dfrac{x[n]+x^*[-n]}{2}`$ | 2.55 (f) |
| Spot values | $`X(e^{j0})=\sum_n x[n]`$, $`X(e^{j\pi})=\sum_n(-1)^n x[n]`$, $`\int_{-\pi}^{\pi}X(e^{j\omega})d\omega=2\pi x[0]`$ | 2.55 |
| Parseval | $`\sum_n\lvert x[n]\rvert^2=\dfrac{1}{2\pi}\int_{-\pi}^{\pi}\lvert X(e^{j\omega})\rvert^2d\omega`$ | 5.37 (unit energy) |
| Sinusoid through a real LTI system | $`\cos(\omega_0 n+\phi)\to\lvert H(e^{j\omega_0})\rvert\cos\left(\omega_0 n+\phi+\angle H(e^{j\omega_0})\right)`$ | 2.41, 2.45, 3.30 |
| Group and phase delay | $`\tau_{gr}(\omega)=-\dfrac{d}{d\omega}\angle H(e^{j\omega})`$ and $`\tau_{ph}(\omega)=-\dfrac{\angle H(e^{j\omega})}{\omega}`$ | 5.19, 5.46, 5.63 |
| Forced zeros of linear-phase FIR | Type II at $`z=-1`$; Type III at $`z=1`$ and $`z=-1`$; Type IV at $`z=1`$ | 5.37, 5.58 |
| Frequency sampling causes time aliasing | $`N`$ samples of $`X(e^{j\omega})`$ give $`\tilde{x}[n]=\sum_{r}x[n+rN]`$ on $`0\le n\le N-1`$ | 8.7, 8.8, 8.32 |
| Circular shift | $`x[((n-m))_N]\longleftrightarrow e^{-j2\pi km/N}X[k]`$ | 8.10 |

---

## 3. Sequences, convolution, and LTI basics (PS1)

### 3.1 Index manipulation (2.21)

Given $`x[n]=1`$ for $`-1\le n\le 3`$, $`x[4]=\tfrac{1}{2}`$, and zero elsewhere.

**Rule:** for $`y[n]=x[f(n)]`$, the sample $`x[m]`$ lands at every $`n`$ with $`f(n)=m`$. For $`y[n]=x[n_0-n]`$ that is $`n=n_0-m`$: reverse, then shift right by $`n_0`$.

| Operation | Nonzero samples |
|---|---|
| $`x[n-2]`$ | $`1`$ for $`1\le n\le 5`$ and $`\tfrac{1}{2}`$ at $`n=6`$ |
| $`x[4-n]`$ | $`\tfrac{1}{2}`$ at $`n=0`$ and $`1`$ for $`1\le n\le 5`$ |
| $`x[2n]`$ | $`1`$ at $`n=0`$ and $`n=1`$, $`\tfrac{1}{2}`$ at $`n=2`$ (odd-index samples are thrown away) |
| $`x[n]u[2-n]`$ | $`1`$ for $`-1\le n\le 2`$ (keeps only $`n\le 2`$) |
| $`x[n-1]\delta[n-3]`$ | $`x[2]\delta[n-3]=\delta[n-3]`$ |

### 3.2 Step response is the running sum (2.29)

$$
s[n]=\sum_{k=-\infty}^{n}h[k]
$$

Here $`h[n]`$ is 1, 1, 1, 1, −2, −2 on $`0\le n\le 5`$.

| Input | Output |
|---|---|
| $`u[n]`$ | $`s[n]`$ = 1, 2, 3, 4, 2, 0 on $`0\le n\le 5`$, and zero for $`n\ge 6`$ because $`\sum h=0`$ |
| $`u[n-4]`$ | $`s[n-4]`$: the same values on $`4\le n\le 9`$ |
| $`u[n]-u[n-4]`$ | $`s[n]-s[n-4]`$ = 1, 2, 3, 4, 1, −2, −3, −4, −2 on $`0\le n\le 8`$ |

### 3.3 Cascades and superposition (2.30)

$`h_1[n]=1`$ on $`0\le n\le 3`$ and $`h_2[n]=1`$ on $`-3\le n\le 0`$.

- **Input $`(-1)^n u[n]`$ through $`h_1`$** (a 4-point running sum): four alternating terms cancel for $`n\ge 3`$, so $`w[n]=\delta[n]+\delta[n-2]`$. Then $`y[n]=h_2[n]+h_2[n-2]`$ = 1, 1, 2, 2, 1, 1 on $`-3\le n\le 2`$.
- **Overall impulse response:** $`h=h_1*h_2`$ = 1, 2, 3, 4, 3, 2, 1 on $`-3\le n\le 3`$. Two length-4 boxes give a length-7 triangle.
- **Input $`2\delta[n]+4\delta[n-4]-2\delta[n-12]`$:** $`w[n]=2h_1[n]+4h_1[n-4]-2h_1[n-12]`$ (separate blocks of height 2, 4, −2) and $`y[n]=2h[n]+4h[n-4]-2h[n-12]`$. The first two terms overlap on $`1\le n\le 3`$, giving 2, 4, 6, 8, 10, 12, 14, 16, 12, 8, 4 on $`-3\le n\le 7`$, then −2, −4, −6, −8, −6, −4, −2 on $`9\le n\le 15`$.

**Support rule for convolution:**

$$
x \text{ on } [N_1,N_2] \text{ and } h \text{ on } [M_1,M_2]\quad\Longrightarrow\quad y=x*h \text{ on } [N_1+M_1,\;N_2+M_2] \text{ with length } L_x+L_h-1
$$

---

## 4. Sinusoids through LTI systems (PS1, PS2, PS3)

Complex exponentials are eigenfunctions of LTI systems:

$$
e^{j\omega_0 n}\;\longrightarrow\;H(e^{j\omega_0})e^{j\omega_0 n}
$$

For a real $`h[n]`$ (conjugate-symmetric $`H`$):

$$
A\cos(\omega_0 n+\phi)\;\longrightarrow\;A\left|H(e^{j\omega_0})\right|\cos\left(\omega_0 n+\phi+\angle H(e^{j\omega_0})\right)
$$

**Recipe**

1. Reduce $`\omega_0`$ into $`(-\pi,\pi]`$ by subtracting multiples of $`2\pi`$. If it lands negative, use $`\cos(-\theta)=\cos\theta`$ to make it positive (this flips the sign of the phase constant too).
2. Read $`|H|`$ and $`\angle H`$ at that frequency, from a formula or a plot.
3. If $`h[n]`$ is complex, meaning $`H(e^{-j\omega})\ne H^*(e^{j\omega})`$, split the cosine into two exponentials and evaluate $`H`$ at $`+\omega_0`$ and $`-\omega_0`$ separately. The output can be complex.

### 4.1 Complex system (2.62)

$`H(e^{j\omega})=e^{-j(\omega/2+\pi/4)}`$ for $`-\pi<\omega\le\pi`$ and $`x[n]=\cos(15\pi n/4-\pi/3)`$.

- $`15\pi/4=4\pi-\pi/4`$, so $`x[n]=\cos(-\pi n/4-\pi/3)=\cos(\pi n/4+\pi/3)`$.
- $`H(e^{j\pi/4})=e^{-j3\pi/8}`$ and $`H(e^{-j\pi/4})=e^{-j\pi/8}`$. These are not conjugates, so $`h[n]`$ is complex.

$$
y[n]=\tfrac{1}{2}e^{j\pi/3}e^{-j3\pi/8}e^{j\pi n/4}+\tfrac{1}{2}e^{-j\pi/3}e^{-j\pi/8}e^{-j\pi n/4}=e^{-j\pi/4}\cos\left(\frac{\pi n}{4}+\frac{5\pi}{24}\right)
$$

> [!TIP]
> If $`H(e^{j\omega})=e^{-j(a\omega+b)}`$ on $`(-\pi,\pi)`$, the linear part is a delay of $`a`$ and the constant $`e^{-jb}`$ multiplies both exponentials, so $`\cos(\omega_0 n+\phi)\to e^{-jb}\cos\left(\omega_0(n-a)+\phi\right)`$. Here $`a=\tfrac{1}{2}`$ and $`b=\tfrac{\pi}{4}`$ give $`e^{-j\pi/4}\cos(\pi n/4-\pi/8+\pi/3)`$, the same answer in one line.

### 4.2 Reading a phase plot (2.41)

$`|H(e^{j\omega})|=1`$ and $`\angle H(e^{j\omega})=-\omega/3-\pi/2`$ for $`0<\omega\le\pi`$; input $`x[n]=\cos(3\pi n/2+\pi/4)`$.

- $`3\pi/2=2\pi-\pi/2`$, so $`x[n]=\cos(-\pi n/2+\pi/4)=\cos(\pi n/2-\pi/4)`$.
- $`\angle H(e^{j\pi/2})=-\pi/6-\pi/2=-2\pi/3`$.
- $`y[n]=\cos(\pi n/2-\pi/4-2\pi/3)=\cos(\pi n/2-11\pi/12)`$, which is the same as $`\cos(3\pi n/2+11\pi/12)`$.

### 4.3 Rational system at one frequency (3.30 c)

Causal $`H(z)=\dfrac{1-z^{-1}}{1-0.25z^{-2}}`$ has poles at $`\pm 0.5`$, so it is stable. For $`x[n]=\cos(\pi n/2)`$:

$$
H(e^{j\pi/2})=\frac{1-(-j)}{1-0.25(-1)}=\frac{1+j}{1.25}=0.8\sqrt{2}e^{j\pi/4}\quad\Longrightarrow\quad y[n]=0.8\sqrt{2}\cos\left(\frac{\pi n}{2}+\frac{\pi}{4}\right)
$$

### 4.4 Mixed input through a cascade (2.45)

First difference $`w[n]=x[n]-x[n-1]`$, then an ideal lowpass filter with $`\omega_c=\pi/2`$ and $`h_2[n]=\dfrac{\sin(\pi n/2)}{\pi n}`$ (with $`h_2[0]=\tfrac{1}{2}`$). Split the input by linearity: sinusoids use $`H(e^{j\omega})`$, impulses and steps use $`h[n]`$.

| Input piece | What happens | Output piece |
|---|---|---|
| $`\cos(0.4\pi n)`$ | $`0.4\pi<\pi/2`$, passes the lowpass filter | $`\cos(0.4\pi n)-\cos(0.4\pi(n-1))=2\sin(0.2\pi)\cos(0.4\pi n+0.3\pi)`$ |
| $`\sin(0.6\pi n)`$ | $`0.6\pi>\pi/2`$, blocked | $`0`$ |
| $`5\delta[n-2]+2u[n]`$ | first difference gives $`2\delta[n]+5\delta[n-2]-5\delta[n-3]`$ | $`2h_2[n]+5h_2[n-2]-5h_2[n-3]`$ |

The closed form in the first row uses $`1-e^{-j\omega}=2j\sin(\omega/2)e^{-j\omega/2}`$, which at $`\omega=0.4\pi`$ is $`2\sin(0.2\pi)e^{j0.3\pi}`$.

---

## 5. DTFT calculations and properties (PS2, PS4)

### 5.1 First-order DTFT (2.32)

$`X(e^{j\omega})=\dfrac{1}{1-ae^{-j\omega}}`$ with $`-1<a<0`$. Multiply top and bottom by the conjugate of the denominator; $`|1-ae^{-j\omega}|^2=1-2a\cos\omega+a^2`$.

$$
\mathrm{Re}\{X(e^{j\omega})\}=\frac{1-a\cos\omega}{1-2a\cos\omega+a^2}\qquad\qquad \mathrm{Im}\{X(e^{j\omega})\}=\frac{-a\sin\omega}{1-2a\cos\omega+a^2}
$$

$$
\left|X(e^{j\omega})\right|=\frac{1}{\sqrt{1-2a\cos\omega+a^2}}\qquad\qquad \angle X(e^{j\omega})=-\tan^{-1}\left(\frac{a\sin\omega}{1-a\cos\omega}\right)
$$

**Sketch facts for $`a<0`$:** highpass shape. $`|X|`$ is smallest at $`\omega=0`$ with value $`1/(1-a)`$ and largest at $`\omega=\pm\pi`$ with value $`1/(1+a)`$; for $`a=-0.5`$ these are $`0.67`$ and $`2`$. The real part and magnitude are even; the imaginary part and phase are odd and positive on $`0<\omega<\pi`$.

### 5.2 Ideal filters in cascade (2.37)

$`H_1(e^{j\omega})=e^{-j\omega}`$ for $`\pi/4<|\omega|\le\pi`$ (zero otherwise) and $`h_2[n]=\dfrac{2\sin(0.5\pi n)}{\pi n}`$, so $`H_2=2`$ for $`|\omega|\le\pi/2`$.

$$
H(e^{j\omega})=H_1(e^{j\omega})H_2(e^{j\omega})=\begin{cases}2e^{-j\omega} & \pi/4<|\omega|\le\pi/2\\ 0 & \text{otherwise in } [-\pi,\pi]\end{cases}
$$

Fast inverse: a bandpass is (lowpass at $`\pi/2`$) minus (lowpass at $`\pi/4`$); then apply the gain of 2 and the delay of 1.

$$
h[n]=\frac{2\sin\left(\frac{\pi}{2}(n-1)\right)-2\sin\left(\frac{\pi}{4}(n-1)\right)}{\pi(n-1)}\quad n\ne 1\qquad\qquad h[1]=2\cdot\tfrac{1}{2}-2\cdot\tfrac{1}{4}=\tfrac{1}{2}
$$

### 5.3 DTFT shortcuts from a stem plot (2.55)

From Fig. P2.55: $`x[-3]=-1`$, $`x[-1]=2`$, $`x[0]=1`$, $`x[1]=2`$, $`x[3]=2`$, $`x[4]=1`$, $`x[5]=2`$, $`x[7]=-1`$. The sequence is symmetric about $`n=2`$, so $`x[n]=g[n-2]`$ with $`g`$ real and even.

| Asked | Trick | Answer |
|---|---|---|
| $`X(e^{j0})`$ | sum of the samples | $`8`$ |
| $`X(e^{j\pi})`$ | sum of $`(-1)^n x[n]`$ | $`-4`$ |
| $`\angle X(e^{j\omega})`$ | $`X=G(e^{j\omega})e^{-j2\omega}`$ with $`G`$ real | $`-2\omega`$ where $`G>0`$ (add $`\pi`$ where $`G<0`$) |
| $`\int_{-\pi}^{\pi}X(e^{j\omega})d\omega`$ | inverse DTFT at $`n=0`$ | $`2\pi x[0]=2\pi`$ |
| sequence with DTFT $`X(e^{-j\omega})`$ | time reversal | $`x[-n]`$ |
| sequence with DTFT $`\mathrm{Re}\{X(e^{j\omega})\}`$ | even part | $`\tfrac{1}{2}\left(x[n]+x[-n]\right)`$ |

> [!NOTE]
> $`X(e^{j\pi})=G(e^{j\pi})e^{-j2\pi}=-4`$ means $`G(e^{j\pi})<0`$, so near $`\omega=\pi`$ the phase is $`-2\omega+\pi`$. Generalized linear phase allows these $`\pi`$ jumps.

---

## 6. z-transform, ROC, and inverse z-transform (PS3, PS4)

### 6.1 ROC rules

| Sequence | ROC |
|---|---|
| right-sided | outside the outermost pole, $`\lvert z\rvert>r_{\max}`$ |
| causal | right-sided **and** includes $`z=\infty`$ (no positive powers of $`z`$) |
| left-sided | inside the innermost pole |
| two-sided | a ring between two poles |
| finite length | the whole plane, except possibly $`z=0`$ or $`z=\infty`$ |

- **Stable** exactly when the ROC contains the unit circle.
- The ROC never contains a pole; its edges are set by poles.

### 6.2 Inverse z-transform by partial fractions

1. Write $`X(z)`$ in powers of $`z^{-1}`$. If the numerator order is at least the denominator order, long-divide first.
2. For each simple pole $`d_k`$, compute the residue:

$$
A_k=\left.\left(1-d_kz^{-1}\right)X(z)\right|_{z=d_k}
$$

3. Use the ROC to choose each term's sequence:

$$
\frac{A_k}{1-d_kz^{-1}}\;\longleftrightarrow\;\begin{cases}A_k d_k^n u[n] & \text{ROC outside } |d_k|\\ -A_k d_k^n u[-n-1] & \text{ROC inside } |d_k|\end{cases}
$$

### 6.3 Full worked example (3.21)

Causal $`H(z)=\dfrac{4+0.25z^{-1}-0.5z^{-2}}{(1-0.25z^{-1})(1+0.5z^{-1})}`$.

| Part | Answer |
|---|---|
| (a) ROC | $`\lvert z\rvert>0.5`$ (causal, so outside the largest pole) |
| (b) Stable? | Yes: the ROC contains the unit circle |
| (c) Difference equation | $`y[n]+0.25y[n-1]-0.125y[n-2]=4x[n]+0.25x[n-1]-0.5x[n-2]`$ |
| (d) $`h[n]`$ | Long division: $`H=4-\dfrac{0.75z^{-1}}{(1-0.25z^{-1})(1+0.5z^{-1})}=4-\dfrac{1}{1-0.25z^{-1}}+\dfrac{1}{1+0.5z^{-1}}`$, so $`h[n]=4\delta[n]-(0.25)^n u[n]+(-0.5)^n u[n]`$ |
| (e) $`Y(z)`$ for $`x=u[-n-1]`$ | $`X(z)=\dfrac{-1}{1-z^{-1}}`$ for $`\lvert z\rvert<1`$, and $`Y=HX`$ with ROC $`0.5<\lvert z\rvert<1`$ |
| (f) $`y[n]`$ | $`Y=\dfrac{-1/3}{1-0.25z^{-1}}+\dfrac{-1/3}{1+0.5z^{-1}}+\dfrac{-10/3}{1-z^{-1}}`$, so $`y[n]=-\tfrac{1}{3}(0.25)^n u[n]-\tfrac{1}{3}(-0.5)^n u[n]+\tfrac{10}{3}u[-n-1]`$ |

> [!TIP]
> Sanity checks: $`h[0]=4-1+1=4=b_0`$. In (f), the pole at $`z=1`$ lies outside the ring $`0.5<|z|<1`$, so its term is left-sided and picks up the minus sign: $`-\left(-\tfrac{10}{3}\right)u[-n-1]`$.

### 6.4 Pole-zero cancellation and inverse systems (3.30 a, b)

Causal $`H(z)=\dfrac{1-z^{-1}}{1-0.25z^{-2}}=\dfrac{1-z^{-1}}{(1-0.5z^{-1})(1+0.5z^{-1})}`$.

- **(a) $`x=u[n]`$:** the zero at $`z=1`$ cancels the input pole, so $`Y=\dfrac{1}{1-0.25z^{-2}}=\dfrac{0.5}{1-0.5z^{-1}}+\dfrac{0.5}{1+0.5z^{-1}}`$ with ROC $`|z|>0.5`$, and $`y[n]=0.5\left[(0.5)^n+(-0.5)^n\right]u[n]`$. That is $`(0.5)^n`$ for even $`n`$ and $`0`$ for odd $`n`$.
- **(b) Want $`y=\delta[n]-\delta[n-1]`$:** $`X=Y/H=1-0.25z^{-2}`$, so $`x[n]=\delta[n]-0.25\delta[n-2]`$.
- **(c)** is in Section 4.3.

### 6.5 Finding H(z) from an input-output pair (3.40)

$`x[n]=u[n]`$ and $`y[n]=(1/2)^{n-1}u[n+1]=4(1/2)^{n+1}u[n+1]`$.

$$
Y(z)=\frac{4z}{1-0.5z^{-1}}\qquad\qquad H(z)=\frac{Y(z)}{X(z)}=\frac{4z(1-z^{-1})}{1-0.5z^{-1}}\qquad |z|>0.5
$$

- Zeros at $`z=0`$ and $`z=1`$, a pole at $`z=0.5`$, and a pole at $`z=\infty`$ from the factor $`z`$.
- $`h[n]=4(0.5)^{n+1}u[n+1]-4(0.5)^n u[n]=4\delta[n+1]-2(0.5)^n u[n]`$.
- **Stable** (ROC contains the unit circle) but **not causal** ($`h[-1]=4`$; equivalently, the ROC does not include $`z=\infty`$).

### 6.6 Time reversal plus shift (3.37)

$`X(z)`$ belongs to a causal $`x[n]`$, with poles at $`-3/4`$ and $`\tfrac{1}{2}\pm j\tfrac{1}{2}`$ and a zero at $`z=0`$. Find $`Y(z)`$ for $`y[n]=x[-n+3]`$.

$$
y[n]=x[-(n-3)]\quad\Longrightarrow\quad Y(z)=z^{-3}X(z^{-1})
$$

- Each finite pole $`p`$ maps to $`1/p`$: $`-3/4\to-4/3`$ and $`\tfrac{1}{2}\pm j\tfrac{1}{2}\to 1\mp j`$.
- The ROC inverts: $`|z|>3/4`$ becomes $`|z|<4/3`$, and with a pole at the origin the ROC is $`0<|z|<4/3`$.
- **Bookkeeping at $`0`$ and $`\infty`$:** $`z\to 1/z`$ swaps them. With 3 poles and 1 zero, $`X(z)`$ has 2 zeros at $`\infty`$. Those move to the origin, and $`z^{-3}`$ adds 3 poles there, so the net result is **one** pole at $`z=0`$ and no finite zeros.
- **Time-domain check:** $`X(z)\sim z^{-2}`$ as $`z\to\infty`$, so $`x[n]`$ starts at $`n=2`$ and $`y[n]=x[3-n]`$ ends at $`n=1`$. The lowest power in $`Y(z)`$ is then $`z^{-1}`$: a single pole at the origin.

> [!WARNING]
> The posted PS3 solution lists a **triple** pole at $`z=0`$. That count is right only if $`X(z)`$ has a triple zero at the origin. With one zero and three poles, as the solution itself lists for Fig. P3.37, the bookkeeping above gives a single pole. Check the figure. The ROC, $`0<|z|<4/3`$, is the same either way.

### 6.7 Initial values from a causal difference equation (3.44)

$`y[n]+\sum_{k=1}^{10}\alpha_k y[n-k]=x[n]+\beta x[n-1]`$, causal.

- At $`n=0`$ with $`x=\delta`$: every $`h[-k]=0`$, so $`h[0]=1`$.
- At $`n=1`$: $`h[1]+\alpha_1 h[0]=\beta`$, so $`\alpha_1=\dfrac{\beta-h[1]}{h[0]}`$.
- For $`h[n]=(0.9)^n\cos(\pi n/4)u[n]`$, the damped-cosine pair gives

$$
H(z)=\frac{1-0.9\cos(\pi/4)z^{-1}}{1-1.8\cos(\pi/4)z^{-1}+0.81z^{-2}}=\frac{1-0.45\sqrt{2}z^{-1}}{1-0.9\sqrt{2}z^{-1}+0.81z^{-2}}
$$

Poles at $`0.9e^{\pm j\pi/4}`$, zeros at $`0`$ and $`0.45\sqrt{2}\approx 0.636`$, ROC $`|z|>0.9`$.

---

## 7. Pole-zero geometry and frequency shifting (PS5)

### 7.1 Multiplying by (−1)ⁿ (5.25)

$$
(-1)^n h[n]=e^{j\pi n}h[n]\;\longleftrightarrow\;H(e^{j(\omega-\pi)})\qquad\qquad (-1)^n g[n]\;\longleftrightarrow\;G(-z)
$$

- **(a)** An ideal zero-phase lowpass filter with $`\omega_c=\pi/4`$ becomes an ideal highpass filter with passband $`3\pi/4\le|\omega|\le\pi`$.
- **(b)** Every pole and zero moves from $`z_i`$ to $`-z_i`$: a rotation by $`\pi`$, which is a reflection through the origin. This works for a complex $`g[n]`$ too, with no conjugate-pair requirement.

### 7.2 Reading magnitude from the pole-zero plot

For $`H(z)=b_0\dfrac{\prod_k(1-c_kz^{-1})}{\prod_k(1-d_kz^{-1})}`$ with equal numbers of factors:

$$
\left|H(e^{j\omega})\right|=|b_0|\frac{\prod_k\left|e^{j\omega}-c_k\right|}{\prod_k\left|e^{j\omega}-d_k\right|}
$$

A zero on the unit circle gives an exact null. A pole near the unit circle gives a peak. A pole sitting just inside next to a zero gives a narrow notch with a flat response elsewhere.

### 7.3 Notch filter (5.28)

Causal

$$
H(z)=\frac{(1-e^{j\pi/3}z^{-1})(1-e^{-j\pi/3}z^{-1})(1+1.1765z^{-1})}{(1-0.9e^{j\pi/3}z^{-1})(1-0.9e^{-j\pi/3}z^{-1})(1+0.85z^{-1})}
$$

- **(a)** The conjugate pairs expand to $`1-z^{-1}+z^{-2}`$ and $`1-0.9z^{-1}+0.81z^{-2}`$. Multiplying out:

$$
y[n]-0.05y[n-1]+0.045y[n-2]+0.6885y[n-3]=x[n]+0.1765x[n-1]-0.1765x[n-2]+1.1765x[n-3]
$$

- **(b)** Zeros at $`e^{\pm j\pi/3}`$ (on the unit circle) and $`-1.1765`$; poles at $`0.9e^{\pm j\pi/3}`$ and $`-0.85`$; ROC $`|z|>0.9`$.
- **(c)** Exact null at $`\omega=\pi/3`$. Away from it, each zero at $`e^{\pm j\pi/3}`$ and its nearby pole nearly cancel.

> [!TIP]
> $`-1.1765=1/(-0.85)`$, so the real zero and pole form an **all-pass pair** with constant magnitude $`1/0.85\approx 1.1765`$. That makes $`|H|`$ flat (about $`1.29`$ at $`\omega=0`$ and $`1.30`$ at $`\omega=\pi`$) apart from the sharp notch at $`\pi/3`$. In general, a pole at $`a`$ with a zero at $`1/a^*`$ gives $`\left|\dfrac{1-z^{-1}/a^*}{1-az^{-1}}\right|=\dfrac{1}{|a|}`$ on the unit circle.

---

## 8. Group delay and phase delay (PS4, PS6, PS7)

$$
\tau_{gr}(\omega)=-\frac{d}{d\omega}\arg H(e^{j\omega})\qquad\qquad \tau_{ph}(\omega)=-\frac{\arg H(e^{j\omega})}{\omega}
$$

Use the continuous (unwrapped) phase. The $`\pi`$ jumps caused by sign changes of a real amplitude do not count toward group delay.

### 8.1 Narrowband input (5.63)

$`x[n]=s[n]\cos(\omega_0 n)`$ with $`s[n]`$ lowpass and narrow compared with $`\omega_0`$:

$$
y[n]\approx\left|H(e^{j\omega_0})\right|s\left[n-\tau_{gr}(\omega_0)\right]\cos\left(\omega_0\left[n-\tau_{ph}(\omega_0)\right]\right)
$$

- **(a)** Phase $`-\phi_0`$ for $`\omega>0`$ (and $`+\phi_0`$ for $`\omega<0`$): $`y[n]=s[n]\cos(\omega_0 n-\phi_0)`$. The envelope is not delayed.
- **(b)** Phase $`-\phi_0-\omega n_d`$: $`y[n]=s[n-n_d]\cos(\omega_0 n-\phi_1)`$ with $`\phi_1=\phi_0+\omega_0 n_d`$.
- **(c)** $`\tau_{gr}=n_d`$ (envelope delay) and $`\tau_{ph}(\omega_0)=\phi_0/\omega_0+n_d`$ (carrier delay), so $`\omega_0\tau_{ph}(\omega_0)=\phi_1`$ and the formula above holds.
- **(d)** A non-integer $`\tau_{gr}`$ shifts the envelope by a fraction of a sample (bandlimited interpolation). A non-integer $`\tau_{ph}`$ moves the carrier's peaks relative to the envelope by a non-integer number of samples.

### 8.2 Group delay from symmetry (5.19)

If $`h[n]`$ is symmetric or antisymmetric about $`n=\alpha`$, then $`\tau_{gr}=\alpha=\dfrac{n_{\text{first}}+n_{\text{last}}}{2}`$.

| System | Nonzero samples | Symmetry | Group delay |
|---|---|---|---|
| $`h_1`$ | 1 at $`n=0`$ and $`n=4`$ | symmetric | 2 |
| $`h_2`$ | −1, 1, 2, 2, 1, −1 on $`-1\le n\le 4`$ | symmetric | 1.5 |
| $`h_3`$ | 1 at $`n=1`$ and −1 at $`n=3`$ | antisymmetric | 2 |
| $`h_4`$ | 1, 0, 3, −1, 2, −1, 3, 0, 1 on $`-1\le n\le 7`$ | symmetric | 3 |
| $`h_5`$ | −1, 2, 0, −2, 1 on $`1\le n\le 5`$ | antisymmetric | 3 |
| $`h_6`$ | eight ones on $`0\le n\le 7`$ | symmetric | 3.5 |

### 8.3 The PS7 filter

$`H(e^{j\omega})=e^{-j\omega}\left(\tfrac{1}{3}+\tfrac{2}{3}\cos\omega\right)`$, so $`\tau_{gr}=1`$ sample.

---

## 9. Generalized linear-phase FIR filters (PS5, PS6)

PS6 convention: $`H(e^{j\omega})=A(e^{j\omega})e^{-j\alpha\omega+j\beta}`$ with $`A`$ real, $`h[n]`$ nonzero only on $`0\le n\le M`$ (length $`M+1`$), $`\alpha=M/2`$, and group delay $`M/2`$.

| Type | Symmetry | Length | $`\beta`$ | $`A(e^{j\omega})`$ | Forced zeros | Cannot be |
|---|---|---|---|---|---|---|
| I | $`h[n]=h[M-n]`$ | odd ($`M`$ even) | $`0`$ | $`\sum_{n=0}^{M/2}a[n]\cos(\omega n)`$ | none | no restriction |
| II | $`h[n]=h[M-n]`$ | even ($`M`$ odd) | $`0`$ | $`\sum_{n=1}^{(M+1)/2}b[n]\cos\left(\omega(n-\tfrac{1}{2})\right)`$ | $`z=-1`$ (at $`\omega=\pi`$) | highpass, bandstop |
| III | $`h[n]=-h[M-n]`$ | odd ($`M`$ even) | $`\pi/2`$ | $`\sum_{n=1}^{M/2}c[n]\sin(\omega n)`$ | $`z=1`$ and $`z=-1`$ | lowpass, highpass, bandstop |
| IV | $`h[n]=-h[M-n]`$ | even ($`M`$ odd) | $`\pi/2`$ | $`\sum_{n=1}^{(M+1)/2}d[n]\sin\left(\omega(n-\tfrac{1}{2})\right)`$ | $`z=1`$ (at $`\omega=0`$) | lowpass, bandstop |

Coefficients: $`a[0]=h[M/2]`$ and $`a[n]=2h[M/2-n]`$; $`b[n]=d[n]=2h[(M+1)/2-n]`$; $`c[n]=2h[M/2-n]`$. Type III also forces the center sample $`h[M/2]=0`$.

### 9.1 Where the forms come from (5.57)

Pair $`h[n]e^{-j\omega n}`$ with $`h[M-n]e^{-j\omega(M-n)}`$ and factor out $`e^{-j\omega M/2}`$:

$$
h[n]\left(e^{j\omega(M/2-n)}\pm e^{-j\omega(M/2-n)}\right)=\begin{cases}2h[n]\cos\left(\omega(M/2-n)\right) & \text{symmetric}\\ 2jh[n]\sin\left(\omega(M/2-n)\right) & \text{antisymmetric}\end{cases}
$$

The factor $`j=e^{j\pi/2}`$ is where $`\beta=\pi/2`$ comes from. An odd $`M`$ makes $`M/2-n`$ a half-integer, which produces the $`n-\tfrac{1}{2}`$ forms.

> [!NOTE]
> PS7 uses the opposite sign convention, $`H(e^{j\omega})=R(\omega)e^{j(\alpha\omega+\beta)}`$ with $`\alpha=-1`$. It is the same filter with the same delay of 1. Check which convention a problem uses before plugging in.

### 9.2 Design from constraints (5.37)

$`H(z)=a+bz^{-1}+cz^{-2}`$ with generalized linear phase, unit energy, $`a\ge 0`$, and $`H(e^{j0})=H(e^{j\pi})=0`$.

- $`a+b+c=0`$ and $`a-b+c=0`$ give $`b=0`$ and $`c=-a`$. Unit energy, $`2a^2=1`$, gives $`a=1/\sqrt{2}`$.
- $`h[n]=\tfrac{1}{\sqrt{2}}\left(\delta[n]-\delta[n-2]\right)`$, a Type III filter.
- $`H(e^{j\omega})=j\sqrt{2}e^{-j\omega}\sin\omega`$, so $`|H(e^{j\omega})|=\sqrt{2}|\sin\omega|`$: nulls at $`0`$ and $`\pm\pi`$, peak $`\sqrt{2}`$ at $`\pm\pi/2`$.

Pattern to recognize: zeros required at both $`z=1`$ and $`z=-1`$ point straight to Type III.

### 9.3 Lowpass to highpass with (−1)ⁿ (5.58)

Since $`(-1)^{M-n}=(-1)^M(-1)^n`$, the symmetry is kept when $`M`$ is even and flipped when $`M`$ is odd:

| Lowpass type | Type after multiplying by $`(-1)^n`$ |
|---|---|
| I | I (symmetric) |
| II | IV (antisymmetric) |
| III | III (antisymmetric) |
| IV | II (symmetric) |

- **Posted answer:** a symmetric highpass needs a Type I or Type IV lowpass.
- **Caveat:** a Type IV filter always has $`H(e^{j0})=0`$, so it cannot be a useful lowpass, and the Type II highpass it maps to has a forced zero at $`\omega=\pi`$. In practice only Type I works. On the exam, give the algebraic answer and state this caveat.

### 9.4 Cascades (5.46)

$`H_1=A_1e^{-j\omega M_1/2}`$ (Type I, $`M_1`$ even) and $`H_2=jA_2e^{-j\omega M_2/2}`$ (Type IV, $`M_2`$ odd):

$$
H(e^{j\omega})=jA_1(e^{j\omega})A_2(e^{j\omega})e^{-j\omega(M_1+M_2)/2}
$$

- Length $`M_1+M_2+1`$, which is even; group delay $`(M_1+M_2)/2`$; antisymmetric (factor $`j`$) with even length, so **Type IV**.
- Rules: orders add and the $`\beta`$ values add. Symmetric with symmetric gives symmetric, symmetric with antisymmetric gives antisymmetric, and antisymmetric with antisymmetric gives symmetric.

### 9.5 Zero locations of real linear-phase FIR filters

Zeros come in sets $`\{z_0,\ z_0^*,\ 1/z_0,\ 1/z_0^*\}`$. Zeros on the unit circle come in conjugate pairs, real zeros come in reciprocal pairs, and $`z=\pm 1`$ can appear alone. This helps with pole-zero sketches.

---

## 10. Minimax FIR design with the alternation theorem (PS7)

**Problem:** a 3-tap causal symmetric lowpass filter (Type I, $`M=2`$) with passband $`0\le\omega\le\pi/3`$ (desired 1) and stopband $`\pi/2\le\omega\le\pi`$ (desired 0), equal weights. Minimize the maximum error.

**Step 1: polynomial form.**

$$
H(e^{j\omega})=h[0]+h[1]e^{-j\omega}+h[0]e^{-j2\omega}=e^{-j\omega}\left(h[1]+2h[0]\cos\omega\right)
$$

With $`x=\cos\omega`$: $`P(x)=b+ax`$ where $`b=h[1]`$ and $`a=2h[0]`$. The order is $`L=1`$.

**Step 2: map the bands.** Because $`\cos`$ is decreasing, the endpoints swap: passband $`\omega\in[0,\pi/3]`$ maps to $`x\in[\tfrac{1}{2},1]`$ and stopband $`\omega\in[\pi/2,\pi]`$ maps to $`x\in[-1,0]`$.

**Step 3: alternation set.** We need $`L+2=3`$ alternations. $`E(x)=D(x)-P(x)`$ is linear on each band, so its extremes are at band edges, and the candidates are $`x\in\{-1,0,\tfrac{1}{2},1\}`$. The set that works is $`\{-1,0,\tfrac{1}{2}\}`$ with signs $`+`$, $`-`$, $`+`$.

**Step 4: solve.**

$$
\begin{aligned}
E(-1)&=0-(b-a)=+\delta\\
E(0)&=0-b=-\delta\\
E(\tfrac{1}{2})&=1-\left(b+\tfrac{a}{2}\right)=+\delta
\end{aligned}
\qquad\Longrightarrow\qquad \delta=\tfrac{1}{3}\qquad b=\tfrac{1}{3}\qquad a=\tfrac{2}{3}
$$

**Step 5: verify** $`|E|\le\delta`$ on both bands. In the stopband, $`E`$ runs from $`+\tfrac{1}{3}`$ to $`-\tfrac{1}{3}`$; in the passband, from $`+\tfrac{1}{3}`$ down to $`0`$. ✓

**Result:** $`h[n]=\{\tfrac{1}{3},\tfrac{1}{3},\tfrac{1}{3}\}`$, a 3-point moving average, with $`A(\omega)=\tfrac{1}{3}+\tfrac{2}{3}\cos\omega`$, maximum error $`\tfrac{1}{3}`$, and group delay 1.

**Why not another set?** Trying $`\{0,\tfrac{1}{2},1\}`$ gives $`\delta=\tfrac{1}{4}`$, $`a=1`$, $`b=\tfrac{1}{4}`$, but then $`E(-1)=a-b=\tfrac{3}{4}>\delta`$. A wrong set always fails the verification step.

### 10.1 General recipe for the exam

1. Write $`A(\omega)`$ as a sum of cosines (Type I: $`\sum a[n]\cos(\omega n)`$), then substitute $`\cos(k\omega)=\Gamma_k(x)`$ to get a polynomial $`P(x)`$ of order $`L=M/2`$.
2. Map the band edges with $`x=\cos\omega`$, remembering that the order flips.
3. Candidate alternation points are the band edges (including $`x=1`$ for $`\omega=0`$ and $`x=-1`$ for $`\omega=\pi`$) and interior points where $`P'(x)=0`$.
4. Pick $`L+2`$ points with alternating error signs and solve the $`L+2`$ linear equations for the $`L+1`$ coefficients and $`\delta`$.
5. Check $`|E(x)|\le\delta`$ on every band. If it fails, swap in the offending point and repeat.
6. Convert back to $`h[n]`$.

With weights $`W`$, the equations become $`W(x_i)\left[D(x_i)-P(x_i)\right]=\pm\delta`$.

**Scaling up:** a 5-tap Type I filter ($`M=4`$) has $`A(\omega)=h[2]+2h[1]\cos\omega+2h[0]\cos 2\omega`$. With $`\cos 2\omega=2x^2-1`$, $`P(x)=(h[2]-2h[0])+2h[1]x+4h[0]x^2`$, so $`L=2`$ and you need 4 alternations.

---

## 11. DFT, frequency sampling, and time aliasing (PS8)

**Core link:** for $`x[n]`$ of length at most $`N`$, $`X[k]=X(e^{j2\pi k/N})`$. If $`x[n]`$ is longer than $`N`$, the inverse DFT of the $`N`$ frequency samples is the time-aliased sequence

$$
\tilde{x}[n]=\sum_{r=-\infty}^{\infty}x[n+rN]\qquad 0\le n\le N-1
$$

### 11.1 Truncated complex exponential (8.6)

$`x[n]=e^{j\omega_0 n}`$ for $`0\le n\le N-1`$.

$$
X(e^{j\omega})=\sum_{n=0}^{N-1}e^{-j(\omega-\omega_0)n}=e^{-j(\omega-\omega_0)(N-1)/2}\frac{\sin\left(N(\omega-\omega_0)/2\right)}{\sin\left((\omega-\omega_0)/2\right)}
$$

$$
X[k]=X(e^{j2\pi k/N})=\frac{1-e^{j\omega_0 N}}{1-e^{j(\omega_0-2\pi k/N)}}\qquad 0\le k\le N-1
$$

If $`\omega_0=2\pi k_0/N`$ lies on the DFT grid, $`X[k]=N\delta[k-((k_0))_N]`$: a single spike. Off the grid, every bin is nonzero (leakage).

### 11.2 Too few frequency samples (8.7)

$`x[n]=1`$ for $`0\le n\le 5`$ (length 6), sampled at 4 frequencies: $`x_1[n]=x[n]+x[n+4]`$ = 2, 2, 1, 1 on $`0\le n\le 3`$.

### 11.3 Infinite-length sequence (8.8)

$`x[n]=(0.5)^n u[n]`$ with 10 frequency samples:

$$
y[n]=\sum_{r=0}^{\infty}(0.5)^{n+10r}=\frac{(0.5)^n}{1-(0.5)^{10}}=\frac{1024}{1023}(0.5)^n\qquad 0\le n\le 9
$$

In general, $`a^n u[n]`$ sampled at $`N`$ frequencies gives $`\dfrac{a^n}{1-a^N}`$ on $`0\le n\le N-1`$.

### 11.4 Circular shift (8.10)

With $`N=8`$, $`x_1`$ = 0, a, b, c, d, e, 0, 0 and $`x_2`$ = d, e, 0, 0, 0, a, b, c. So $`x_2[n]=x_1[((n-4))_8]`$ and

$$
X_2[k]=e^{-j2\pi k\cdot 4/8}X_1[k]=e^{-j\pi k}X_1[k]=(-1)^kX_1[k]
$$

### 11.5 DTFT samples of a length-P sequence from one N-point DFT (8.32)

| Case | Build $`\tilde{x}[n]`$ on $`0\le n\le N-1`$ | Why it works |
|---|---|---|
| $`N>P`$ | zero-pad: $`x[n]`$ followed by $`N-P`$ zeros | the extra terms are zero, so the DFT sum equals the DTFT sum |
| $`N<P`$ | wrap: $`\tilde{x}[n]=\sum_{r}x[n+rN]`$ | $`e^{-j2\pi k(n+rN)/N}=e^{-j2\pi kn/N}`$ |

**Other DFT facts worth knowing (not on the sheet):**

$$
x[((n-m))_N]\longleftrightarrow e^{-j2\pi km/N}X[k]\qquad e^{j2\pi k_0 n/N}x[n]\longleftrightarrow X[((k-k_0))_N]\qquad x_1[n]\circledast_N x_2[n]\longleftrightarrow X_1[k]X_2[k]
$$

---

## 12. On the sheet but not in PS 1-8

**Bilinear transformation.** With the sheet's form ($`T_d=2`$):

$$
s=\left.\frac{1-z^{-1}}{1+z^{-1}}\right|_{z=e^{j\omega}}=\frac{e^{j\omega/2}-e^{-j\omega/2}}{e^{j\omega/2}+e^{-j\omega/2}}=j\tan\frac{\omega}{2}\quad\Longrightarrow\quad \Omega=\tan\frac{\omega}{2}\qquad \omega=2\tan^{-1}\Omega
$$

To design: prewarp the digital band edges with $`\Omega=\tan(\omega/2)`$, design $`H_a(s)`$, then substitute. The $`j\Omega`$ axis maps onto the unit circle and the left half-plane maps inside it, so stable analog filters give stable digital filters.

**Downsampling by $`M`$.** $`Y(e^{j\omega})`$ is $`M`$ stretched copies of $`X`$ spaced $`2\pi`$ apart, scaled by $`1/M`$. There is no aliasing if $`X(e^{j\omega})=0`$ for $`\pi/M\le|\omega|\le\pi`$. For $`M=2`$:

$$
Y(e^{j\omega})=\frac{1}{2}\left[X(e^{j\omega/2})+X(-e^{j\omega/2})\right]
$$

This is the frequency-domain view of $`x[2n]`$ from 2.21 (c).

---

## 13. Answer key by problem

**PS1**

| Problem | Key idea | Final answer |
|---|---|---|
| 2.21 | index mapping | table in Section 3.1 |
| 2.29 | step response is the running sum of $`h`$ | (a) 1, 2, 3, 4, 2, 0 on $`0\le n\le 5`$; (b) the same delayed by 4; (c) 1, 2, 3, 4, 1, −2, −3, −4, −2 on $`0\le n\le 8`$ |
| 2.30 | cascade and superposition | (a) $`w=\delta[n]+\delta[n-2]`$ and $`y`$ = 1, 1, 2, 2, 1, 1 on $`-3\le n\le 2`$; (b) $`h`$ = 1, 2, 3, 4, 3, 2, 1 on $`-3\le n\le 3`$; (c) $`2h_1[n]+4h_1[n-4]-2h_1[n-12]`$; (d) $`2h[n]+4h[n-4]-2h[n-12]`$ |
| 2.62 | complex $`h`$, so evaluate at $`\pm\omega_0`$ | $`y[n]=e^{-j\pi/4}\cos(\pi n/4+5\pi/24)`$ |

**PS2**

| Problem | Key idea | Final answer |
|---|---|---|
| 2.32 | multiply by the conjugate | formulas in Section 5.1 |
| 2.37 | product of ideal responses | $`H=2e^{-j\omega}`$ on $`\pi/4<\lvert\omega\rvert\le\pi/2`$; $`h[n]=\dfrac{2\sin(\pi(n-1)/2)-2\sin(\pi(n-1)/4)}{\pi(n-1)}`$ with $`h[1]=\tfrac{1}{2}`$ |
| 2.41 | reduce the frequency, read the plot | $`y[n]=\cos(\pi n/2-11\pi/12)`$ |
| 2.45 | one component at a time | $`y[n]=\cos(0.4\pi n)-\cos(0.4\pi(n-1))+2h_2[n]+5h_2[n-2]-5h_2[n-3]`$ |

**PS3**

| Problem | Key idea | Final answer |
|---|---|---|
| 3.21 | ROC and partial fractions | $`\lvert z\rvert>0.5`$; stable; $`h=4\delta[n]-(0.25)^n u[n]+(-0.5)^n u[n]`$; $`y=-\tfrac{1}{3}(0.25)^n u[n]-\tfrac{1}{3}(-0.5)^n u[n]+\tfrac{10}{3}u[-n-1]`$ |
| 3.30 | cancellation, inverse system, sinusoid | (a) $`0.5\left[(0.5)^n+(-0.5)^n\right]u[n]`$; (b) $`x=\delta[n]-0.25\delta[n-2]`$; (c) $`0.8\sqrt{2}\cos(\pi n/2+\pi/4)`$ |
| 3.37 | $`z\to 1/z`$, then $`z^{-3}`$ | poles at $`-4/3`$, $`1\pm j`$, and the origin; ROC $`0<\lvert z\rvert<4/3`$ (see the warning in Section 6.6) |
| 3.40 | $`H=Y/X`$ | $`H=\dfrac{4z(1-z^{-1})}{1-0.5z^{-1}}`$; $`h=4\delta[n+1]-2(0.5)^n u[n]`$; stable and noncausal |

**PS4**

| Problem | Key idea | Final answer |
|---|---|---|
| 2.55 | spot values and symmetry | $`8`$; $`-4`$; $`-2\omega`$; $`2\pi`$; $`x[-n]`$; $`\tfrac{1}{2}(x[n]+x[-n])`$ |
| 3.44 | evaluate the difference equation at $`n=0`$ and $`n=1`$ | $`h[0]=1`$; $`\alpha_1=(\beta-h[1])/h[0]`$; poles $`0.9e^{\pm j\pi/4}`$, ROC $`\lvert z\rvert>0.9`$ |
| 5.63 | envelope delay vs carrier delay | $`y[n]=s[n-\tau_{gr}(\omega_0)]\cos\left(\omega_0[n-\tau_{ph}(\omega_0)]\right)`$ |

**PS5**

| Problem | Key idea | Final answer |
|---|---|---|
| 5.25 | $`(-1)^n`$ shifts by $`\pi`$ | highpass with passband $`3\pi/4\le\lvert\omega\rvert\le\pi`$; poles and zeros move $`z\to -z`$ |
| 5.28 | pole-zero geometry | difference equation in Section 7.3; ROC $`\lvert z\rvert>0.9`$; notch at $`\pi/3`$ |
| 5.37 | constraints point to Type III | $`h=\tfrac{1}{\sqrt{2}}(\delta[n]-\delta[n-2])`$; $`\lvert H\rvert=\sqrt{2}\lvert\sin\omega\rvert`$ |
| 5.58 | parity of $`(-1)^{M-n}`$ | Type I or Type IV (only Type I is a usable lowpass) |

**PS6**

| Problem | Key idea | Final answer |
|---|---|---|
| 5.19 | center of symmetry | 2, 1.5, 2, 3, 3, 3.5 |
| 5.46 | multiply the responses | $`H=jA_1A_2e^{-j\omega(M_1+M_2)/2}`$; length $`M_1+M_2+1`$; delay $`(M_1+M_2)/2`$; Type IV |
| 5.57 | pair symmetric terms | $`\alpha=M/2`$ for all types; $`\beta=0`$ for I and II, $`\beta=\pi/2`$ for III and IV |

**PS7**

| Problem | Key idea | Final answer |
|---|---|---|
| minimax 3-tap lowpass | alternation theorem with $`x=\cos\omega`$ | $`h[n]=\{\tfrac{1}{3},\tfrac{1}{3},\tfrac{1}{3}\}`$; $`\delta=\tfrac{1}{3}`$; group delay 1 |

**PS8**

| Problem | Key idea | Final answer |
|---|---|---|
| 8.6 | finite geometric sum | Dirichlet kernel centered at $`\omega_0`$; $`X[k]=N\delta[k-k_0]`$ when $`\omega_0=2\pi k_0/N`$ |
| 8.7 | time aliasing | 2, 2, 1, 1 on $`0\le n\le 3`$ |
| 8.8 | aliasing an infinite sequence | $`\tfrac{1024}{1023}(0.5)^n`$ on $`0\le n\le 9`$ |
| 8.10 | circular shift by 4 | $`X_2[k]=(-1)^kX_1[k]`$ |
| 8.32 | zero-pad or wrap | $`N>P`$: pad with zeros; $`N<P`$: $`\tilde{x}[n]=\sum_r x[n+rN]`$ |

---

## 14. Exam traps checklist

- [ ] Reduce $`\omega_0`$ into $`(-\pi,\pi]`$ before reading $`H`$ or a plot (2.41, 2.62).
- [ ] If $`H(e^{-j\omega})\ne H^*(e^{j\omega})`$, treat $`\pm\omega_0`$ separately; the output may be complex (2.62).
- [ ] The sheet's difference equation has $`+a_k`$ on the right side.
- [ ] Long-divide before partial fractions when the numerator order is at least the denominator order (3.21).
- [ ] Poles outside the ROC give left-sided terms with a minus sign: $`-A_k d_k^n u[-n-1]`$ (3.21 f).
- [ ] Causal needs the ROC to include $`\infty`$. A factor of $`z`$ means noncausal even when stable (3.40).
- [ ] $`z\to 1/z`$ swaps $`0`$ and $`\infty`$, so count the poles and zeros there (3.37).
- [ ] Pole-zero cancellation can enlarge the ROC (3.30 a).
- [ ] Linear phase is $`-\alpha\omega+\beta`$ only where $`A>0`$; add $`\pi`$ where $`A<0`$ (2.55: $`X(e^{j\pi})=-4`$).
- [ ] Group delay ignores $`\pi`$ jumps and equals the center of symmetry (5.19).
- [ ] Type II has a zero at $`\omega=\pi`$, Type III at $`0`$ and $`\pi`$, Type IV at $`0`$ (5.37, 5.58).
- [ ] Multiplying by $`(-1)^n`$: $`\omega\to\omega-\pi`$, $`z\to -z`$, and types map I→I, II→IV, III→III, IV→II (5.25, 5.58).
- [ ] Two sign conventions for $`\alpha`$ in linear phase (PS6 vs PS7).
- [ ] $`x=\cos\omega`$ flips the interval order: $`[0,\omega_p]\to[\cos\omega_p,1]`$ (PS7).
- [ ] An order-$`L`$ polynomial needs $`L+2`$ alternations; always verify $`|E|\le\delta`$ on every band (PS7).
- [ ] An $`N`$-point DFT samples $`X(e^{j\omega})`$ at $`2\pi k/N`$; fewer samples than the signal length causes time aliasing (8.7, 8.8).
- [ ] The DFT shift property is circular, not linear (8.10).
- [ ] Leakage appears unless $`\omega_0`$ is exactly on the DFT grid (8.6).

---

## 15. Rapid self-check

Try these first; the answers follow.

1. Causal $`H(z)=\dfrac{1+2z^{-1}}{1-0.5z^{-1}}`$. Find $`h[n]`$ with partial fractions.
2. A real LTI system has $`H(e^{j\omega})=e^{-j2\omega}`$ for $`|\omega|<\pi/2`$ and $`0`$ for $`\pi/2\le|\omega|\le\pi`$. Find $`y[n]`$ for $`x[n]=\cos(7\pi n/4)+\cos(3\pi n/4)`$.
3. $`h[n]`$ = 1, 2, 0, −2, −1 on $`0\le n\le 4`$. Which type is it, what is its group delay, and can it be a highpass filter?
4. A Type II filter with $`M_1=3`$ is cascaded with a Type III filter with $`M_2=4`$. What are the overall type, length, and group delay?
5. How many alternations does the optimal 7-tap Type I lowpass filter need?
6. $`G(z)`$ has a pole at $`0.9e^{j\pi/6}`$. Where is the matching pole of the z-transform of $`(-1)^n g[n]`$?
7. $`x[n]=(0.8)^n u[n]`$. Take 8 samples of $`X(e^{j\omega})`$ at $`\omega=2\pi k/8`$ and invert them with an 8-point inverse DFT. What do you get?
8. $`H(z)=\dfrac{z}{1-0.5z^{-1}}`$ with ROC $`|z|>0.5`$. Is it causal? Is it stable?
9. $`x[n]`$ has length 12. How do you get $`X(e^{j2\pi k/8})`$ for $`0\le k\le 7`$ from one 8-point DFT?
10. Use the sheet to write $`Y(e^{j\omega})`$ for $`y[n]=x[2n]`$.

### Answers

1. Long division gives $`H=-4+\dfrac{5}{1-0.5z^{-1}}`$, so $`h[n]=-4\delta[n]+5(0.5)^n u[n]`$. Check: $`h[0]=1`$.
2. $`7\pi/4\equiv-\pi/4`$, so the first term is $`\cos(\pi n/4)`$, which passes with phase $`-\pi/2`$ and becomes $`\sin(\pi n/4)`$. The $`3\pi/4`$ term is blocked. So $`y[n]=\sin(\pi n/4)`$.
3. Antisymmetric with odd length, so Type III; group delay 2. It cannot be a highpass filter because of the forced zero at $`z=-1`$.
4. The $`\beta`$ values add to $`\pi/2`$ (antisymmetric) and $`M=7`$ is odd (length 8), so Type IV with group delay 3.5.
5. $`M=6`$, so $`L=3`$ and the filter needs $`L+2=5`$ alternations.
6. $`-0.9e^{j\pi/6}=0.9e^{j7\pi/6}`$.
7. $`\dfrac{(0.8)^n}{1-(0.8)^8}\approx 1.2016(0.8)^n`$ on $`0\le n\le 7`$.
8. $`h[n]=(0.5)^{n+1}u[n+1]`$, so $`h[-1]=1`$: not causal, but stable because the ROC contains the unit circle.
9. Form $`\tilde{x}[n]=x[n]+x[n+8]`$ for $`0\le n\le 3`$ and $`\tilde{x}[n]=x[n]`$ for $`4\le n\le 7`$, then take its 8-point DFT.
10. $`Y(e^{j\omega})=\tfrac{1}{2}\left[X(e^{j\omega/2})+X(e^{j(\omega-2\pi)/2})\right]`$.
