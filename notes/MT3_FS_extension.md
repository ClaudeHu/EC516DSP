# EC516 Midterm 1: What to Remember That Is Not on the Formula Sheet

**Exam structure (from the announcement):** Q1 from Problem Sets 1–3 (40%), Q2 from lectures up to Sept 30, i.e. Lectures 1–8 (40%), Q3 a challenge problem (20%).

**How this note is built:** everything below is a rule, theorem, property or procedure that appears in the lectures or problem-set solutions and is **not** printed on the formula sheet. Source tags: `L3` = Lecture 3, `PS 3.21` = that problem.

> **Gap you need to fill yourself.** The Lecture 7 and Lecture 8 PDFs are whiteboard photos, and only the **first** photo in each file actually exported. Lecture 7 has 4 broken image placeholders and Lecture 8 has about 15. Section 9 covers what is visible plus the earlier OneNote lectures; anything else from Sept 28 and Sept 30 has to come from your own notes or a re-export from the instructor.

**Contents**

0. [What the sheet already gives you, and three places it can mislead](#0-what-the-sheet-already-gives-you-and-three-places-it-can-mislead)
1. [Signals and index operations](#1-signals-and-index-operations)
2. [LTI systems and convolution](#2-lti-systems-and-convolution)
3. [Frequency response: gain and delay](#3-frequency-response-gain-and-delay)
4. [DTFT](#4-dtft)
5. [z-transform and ROC](#5-z-transform-and-roc)
6. [Poles and zeros](#6-poles-and-zeros)
7. [Filters: system function, difference equations, FIR and IIR](#7-filters-system-function-difference-equations-fir-and-iir)
8. [Getting the impulse response](#8-getting-the-impulse-response)
9. [Phase: envelope, fractional delay, phase delay, linear phase, group delay](#9-phase-envelope-fractional-delay-phase-delay-linear-phase-group-delay)
10. [Algebra you are expected to do fast](#10-algebra-you-are-expected-to-do-fast)
11. [Recipes by question type](#11-recipes-by-question-type)
12. [Traps](#12-traps)
13. [Coverage map and errata](#13-coverage-map-and-errata)

---

## 0. What the sheet already gives you, and three places it can mislead

**Do not spend memory on these (they are printed):** definitions of $u[n]$ and $\delta[n]$; Euler's formulas; DTFT and inverse DTFT; the convolution sum; finite and infinite geometric sums; DTFT properties (time shift, frequency shift, conjugation, time reversal, convolution); DTFT pairs (complex exponential, shifted impulse, $N$-point box, sinc/ideal lowpass); z-transform definition; z-transform properties (shift, time reversal, conjugation, convolution); the pair $a^n u[n]$ with its ROC; recursive and non-recursive difference equations; definitions of FIR and IIR.

**Three places to be careful:**

1. **The complex-exponential pair.** As printed, the sheet reads $e^{j\omega_0 n}\Leftrightarrow 2\pi\sum_k\delta(\omega-k\omega_0)$. The DTFT of $e^{j\omega_0 n}$ is an impulse at $\omega_0$ repeated every $2\pi$, which is what Lecture 2 wrote. Use this:

$$e^{j\omega_0 n}\;\Longleftrightarrow\;2\pi\sum_{k=-\infty}^{\infty}\delta(\omega-\omega_0-2\pi k)$$

2. **The sign convention of the recursive difference equation.** The sheet (and Lecture 5) put the feedback terms on the right:

$$y[n]=\sum_{k=1}^{N}a_k y[n-k]+\sum_{m=0}^{M}b_m x[n-m]\quad\Longleftrightarrow\quad H(z)=\frac{\sum_{m=0}^{M}b_m z^{-m}}{1-\sum_{k=1}^{N}a_k z^{-k}}$$

The denominator coefficients have the **opposite sign** to the $a_k$. The textbook problems in PS3 put the $y$ terms on the left instead, so always move everything to one side before reading coefficients.

3. **The z-transform properties are printed without ROCs, and the sheet has no ROC rules and no left-sided pair.** Section 5 is all memory work.

---

## 1. Signals and index operations

`PS 2.21, 2.45`

$$\delta[n]=u[n]-u[n-1],\qquad u[n]=\sum_{k=-\infty}^{n}\delta[k],\qquad x[n]=\sum_{k=-\infty}^{\infty}x[k]\delta[n-k]$$

| Expression | Effect |
|---|---|
| $x[n-n_0]$ | Shift right by $n_0$ |
| $x[-n]$ | Flip about $n=0$ |
| $x[n_0-n]$ | Flip, then shift right by $n_0$ |
| $x[Mn]$ | Keep every $M$-th sample, discard the rest |
| $x[n]u[n_0-n]$ | Keep $n\le n_0$, zero the rest |
| $x[n]\delta[n-n_0]$ | One sample: $x[n_0]\delta[n-n_0]$ |
| $x[n]*\delta[n-n_0]$ | The whole sequence shifted: $x[n-n_0]$ |

**Check that never fails:** the sample at index $m$ of $x$ lands where the argument equals $m$. For $x[4-n]$, solve $4-n=m$.

**Frequencies only exist modulo $2\pi$** `PS 2.62, 2.41`. Since $n$ is an integer, $e^{j(\omega_0+2\pi k)n}=e^{j\omega_0 n}$. Reduce every input frequency into $(-\pi,\pi]$ before reading $H(e^{j\omega})$. If the reduced frequency is negative, $\cos(-\theta)=\cos\theta$ flips the sign of the phase constant too:

$$\cos\!\left(\tfrac{3\pi}{2}n+\tfrac{\pi}{4}\right)=\cos\!\left(-\tfrac{\pi}{2}n+\tfrac{\pi}{4}\right)=\cos\!\left(\tfrac{\pi}{2}n-\tfrac{\pi}{4}\right)$$

Two sequences to recognize on sight `L2`: $e^{j0n}=1$ (lowest frequency) and $e^{j\pi n}=(-1)^n$ (highest frequency).

---

## 2. LTI systems and convolution

`L1, PS 2.29, 2.30, 2.45`

### 2.1 Where the convolution sum comes from

The derivation is three steps, and each step uses one property `L1`:

1. Time invariance: $\delta[n-k]\to h[n-k]$
2. Scaling: $x[k]\delta[n-k]\to x[k]h[n-k]$
3. Additivity: summing over $k$ gives $x[n]\to\sum_k x[k]h[n-k]$

An LTI system is fully described by $h[n]$. The lecture calls $\sum_k h[k]x[n-k]$ the **standard form**: the output is a sum of scaled, delayed echoes of the input.

### 2.2 The shift rule for convolution (differs from multiplication)

$$\text{If } y=x*h:\qquad x[n-a]*h[n-b]=y[n-a-b]$$

Shifts **add**. For multiplication, shifting both factors by $n_0$ shifts the product by $n_0$; for convolution it shifts the result by $2n_0$. Lecture examples: $x[n-3]*h[n+3]=y[n]$ and $x[n-7]*h[n+5]=y[n-2]$.

### 2.3 Properties

| Property | Statement | Use |
|---|---|---|
| Commutative | $x * h=h * x$ | Flip whichever is easier; the input and impulse response can swap roles |
| Associative | $(x * h_1) * h_2=x * (h_1 * h_2)$ | Cascade: $h=h_1 * h_2$, order irrelevant |
| Distributive | $x * (h_1+h_2)=x * h_1+x * h_2$ | Parallel: $h=h_1+h_2$ |
| Identity | $x * \delta=x$ | |
| Running sum | $x[n] * u[n]=\sum_{k\le n}x[k]$ | Step response $s[n]=\sum_{k\le n}h[k]$ and $h[n]=s[n]-s[n-1]$ |

### 2.4 Finite-length facts and shortcuts

- **Support adds:** $x$ on $[N_1,N_2]$ and $h$ on $[M_1,M_2]$ gives $y$ on $[N_1+M_1, N_2+M_2]$. Length is $L_x+L_h-1$.
- **Sums multiply:** $\sum y=\left(\sum x\right)\left(\sum h\right)$.
- Two length-$N$ boxes convolve to a triangle of length $2N-1$ with peak $N$.
- **Mechanics for short inputs** `L1`: write $y[n]=\sum_k x[k]h[n-k]$ as one scaled, shifted copy of $h$ per input sample, stack the copies in rows, add the columns.
- **Reuse a known response** `PS 2.29`: if $x_a\to y_a$ then $\sum_i c_i x_a[n-n_i]\to\sum_i c_i y_a[n-n_i]$. A box is $u[n]-u[n-N]$, so its response is $s[n]-s[n-N]$.

### 2.5 System properties

| Property | Definition | Test for LTI |
|---|---|---|
| Causal | Output depends only on present and past input | $h[n]=0$ for $n<0$ |
| Stable (BIBO) | Bounded input gives bounded output | $\sum_n\lvert h[n]\rvert<\infty$ `L3` |

**Course-wide assumption** `L4`: filters are causal unless a problem says otherwise, so $y[n]=\sum_{k=0}^{\infty}h[k]x[n-k]$.

---

## 3. Frequency response: gain and delay

`L1, L2, L5, L6, PS 2.62, 2.41, 2.45, 3.30`

### 3.1 LTI systems preserve frequency

$$e^{j\omega_0 n}\;\longrightarrow\;H(e^{j\omega_0})e^{j\omega_0 n},\qquad H(e^{j\omega})=\sum_{k}h[k]e^{-j\omega k}$$

The input must be the exponential **for all** $n$. $H(e^{j\omega_0})$ is a complex number that does not depend on $n$. The frequency response is the DTFT of the impulse response.

**Terminology** `L3`: $h[n]$ impulse response, $H(e^{j\omega})$ frequency response, $H(z)$ system function (transfer function).

### 3.2 Gain and delay form

$$H(e^{j\omega_0})e^{j\omega_0 n}=\underbrace{\lvert H(e^{j\omega_0})\rvert}_{\text{gain}}\exp\!\Big(j\omega_0\big(n-\underbrace{\tau_{ph}}_{\text{delay}}\big)\Big),\qquad\tau_{ph}=-\frac{\angle H(e^{j\omega_0})}{\omega_0}$$

Magnitude controls the gain at each frequency; phase controls the delay at each frequency. The delay is generally **not an integer** (fractional delay, Section 9).

### 3.3 Real sinusoid in

If $h[n]$ is real then $H(e^{-j\omega})=H^*(e^{j\omega})$, so $\lvert H\rvert$ is even and $\angle H$ is odd `L5`, and

$$A\cos(\omega_0 n+\phi)\;\longrightarrow\;A\lvert H(e^{j\omega_0})\rvert\cos\!\big(\omega_0 n+\phi+\angle H(e^{j\omega_0})\big)$$

If $H$ is **not** conjugate-symmetric (for example a phase like $-\tfrac{\omega}{2}-\tfrac{\pi}{4}$ for all $\omega$, which is not odd), this shortcut is invalid `PS 2.62`. Split the cosine with Euler, multiply each exponential by its own $H(e^{\pm j\omega_0})$, and add. The output is generally complex.

### 3.4 Interconnections

$$\text{Cascade: }H=H_1H_2\qquad\text{Parallel: }H=H_1+H_2$$

For piecewise (ideal) filters multiply band by band: the cascade is nonzero only where both are nonzero; magnitudes multiply and phases add `PS 2.37`.

### 3.5 Ideal filters (the sheet has only the lowpass pair)

| Filter | $H(e^{j\omega})$ on $\lvert\omega\rvert\le\pi$ | $h[n]$ |
|---|---|---|
| Lowpass, cutoff $\omega_c$ | $1$ for $\lvert\omega\rvert\le\omega_c$ | $\dfrac{\sin\omega_c n}{\pi n}$, with $h[0]=\dfrac{\omega_c}{\pi}$ |
| Highpass | $1-H_{lp}$ | $\delta[n]-\dfrac{\sin\omega_c n}{\pi n}$ |
| Bandpass $\omega_1<\lvert\omega\rvert\le\omega_2$ | $H_{lp,\omega_2}-H_{lp,\omega_1}$ | $\dfrac{\sin\omega_2 n-\sin\omega_1 n}{\pi n}$ |
| Any of these with gain $G$ and delay $n_d$ | multiply by $Ge^{-j\omega n_d}$ | multiply by $G$, replace $n$ with $n-n_d$ |

- Read gain and cutoff straight off a sinc: $\dfrac{2\sin(0.5\pi n)}{\pi n}$ is gain 2, cutoff $0.5\pi$ `PS 2.37`.
- A sinusoid in the passband keeps the passband gain and phase; one in the stopband disappears `PS 2.45`.
- **The $0/0$ sample:** at $n=n_d$ evaluate the limit. Example from `PS 2.37`: $h[1]=2(0.5)-2(0.25)=0.5$.

### 3.6 Two filters worth knowing cold

**First difference** $y[n]=x[n]-x[n-1]$ `PS 2.45`: $H=1-e^{-j\omega}$, zero gain at DC, turns a step into an impulse.

**$N$-point box (moving sum)** $h[n]=u[n]-u[n-N]$ `L2, L3, PS 2.30`: the sheet gives its DTFT. What to add from memory:

- DC gain $N$; nulls at $\omega=\dfrac{2\pi k}{N}$ for $k\ne0$.
- The ratio $\dfrac{\sin(\omega N/2)}{\sin(\omega/2)}$ is **real but not positive everywhere**, so it is not the magnitude. Where it goes negative the true phase jumps by $\pi$ on top of the linear term $-\omega\tfrac{N-1}{2}$.
- A null kills the steady state: the 4-point box has a null at $\omega=\pi$, so $(-1)^n u[n]$ gives only start-up samples, $w[n]=\delta[n]+\delta[n-2]$.

### 3.7 Input for all $n$ versus input switched on at $n=0$

$e^{j\omega_0 n}$ for all $n$ gives exactly $H(e^{j\omega_0})e^{j\omega_0 n}$. With $u[n]$ attached, a stable causal system gives that steady state **plus a transient**.

---

## 4. DTFT

`L2, L3, PS 2.32, 2.37`

### 4.1 Properties missing from the sheet

| Property | Time | Frequency |
|---|---|---|
| Periodicity | any $x[n]$ | $X(e^{j\omega})=X(e^{j(\omega-2\pi)})$ |
| Linearity | $\alpha_1x_1[n]+\alpha_2x_2[n]$ | $\alpha_1X_1+\alpha_2X_2$ |
| Multiplication (dual of convolution) | $x_1[n]x_2[n]$ | $\dfrac{1}{2\pi}\displaystyle\int_{-\pi}^{\pi}X_1(e^{j\theta})X_2(e^{j(\omega-\theta)})d\theta$ |

**Duals** `L2`: time shift and frequency shift are a dual pair; convolution in time and multiplication in time are a dual pair. A special case of frequency shift: $(-1)^n x[n]\Leftrightarrow X(e^{j(\omega-\pi)})$, which turns a lowpass into a highpass.

### 4.2 Symmetry for real $x[n]$

$$X(e^{-j\omega})=X^*(e^{j\omega})\quad\Longrightarrow\quad\mathrm{Re}\{X\},\lvert X\rvert\text{ even};\qquad\mathrm{Im}\{X\},\angle X\text{ odd}$$

Odd functions pass through zero at $\omega=0$. To sketch on $[-\pi,\pi]$: get the values at $0$ and $\pi$, then apply the symmetry.

### 4.3 Convergence

- **Dirichlet condition** `L3`: $X(e^{j\omega})$ converges when $x[n]$ is absolutely summable, $\sum_n\lvert x[n]\rvert<\infty$.
- Same condition as stability: an LTI system is stable exactly when $h[n]$ is absolutely summable, and then its frequency response exists.
- Link to the z-transform: $X(e^{j\omega})=X(z)\big\rvert_{z=e^{j\omega}}$ **provided the unit circle is in the ROC**.

### 4.4 Pairs to add to the sheet's list

| $x[n]$ | $X(e^{j\omega})$ | How to get it from the sheet |
|---|---|---|
| $\delta[n]$ | $1$ | shifted impulse with $n_0=0$ |
| $1$ | $2\pi\sum_k\delta(\omega-2\pi k)$ | exponential with $\omega_0=0$ |
| $\cos(\omega_0 n+\phi)$ | $\pi e^{j\phi}\delta(\omega-\omega_0)+\pi e^{-j\phi}\delta(\omega+\omega_0)$ per period | Euler plus linearity |
| $a^n u[n]$, $\lvert a\rvert<1$ | $\dfrac{1}{1-ae^{-j\omega}}$ | infinite sum formula, or $z=e^{j\omega}$ in the z pair |

### 4.5 The "cute trick" for anything of the form $1-e^{-j\theta}$

`L2` Pull out half the exponent so a sine is left:

$$1-e^{-j\omega N}=e^{-j\omega N/2}\left(e^{j\omega N/2}-e^{-j\omega N/2}\right)=2j\sin(\omega N/2)e^{-j\omega N/2}$$

$$1+e^{-j\omega}=2\cos(\omega/2)e^{-j\omega/2}$$

This is how the box DTFT is derived from the finite sum formula; be able to reproduce it.

### 4.6 Real part, imaginary part, magnitude, phase of a first-order term

`PS 2.32` Multiply top and bottom by the conjugate of the denominator. With $D(\omega)=1-2a\cos\omega+a^2$:

$$\frac{1}{1-ae^{-j\omega}}=\frac{(1-a\cos\omega)-ja\sin\omega}{D(\omega)},\qquad\lvert X\rvert=\frac{1}{\sqrt{D(\omega)}},\qquad\angle X=-\tan^{-1}\!\frac{a\sin\omega}{1-a\cos\omega}$$

- Value at $\omega=0$ is $\dfrac{1}{1-a}$; at $\omega=\pi$ it is $\dfrac{1}{1+a}$.
- $0<a<1$ is lowpass; $-1<a<0$ is highpass.

### 4.7 Inverse DTFT of a piecewise response

`PS 2.37` First try to recognize lowpass pieces, delays and gains (Section 3.5). If you must integrate and the bands are symmetric with a linear-phase factor:

$$h[n]=\frac{G}{\pi}\int_{\omega_1}^{\omega_2}\cos\!\big(\omega(n-n_d)\big)d\omega=\frac{G\big[\sin\omega_2(n-n_d)-\sin\omega_1(n-n_d)\big]}{\pi(n-n_d)}$$

State the $n=n_d$ sample separately: $h[n_d]=\dfrac{G(\omega_2-\omega_1)}{\pi}$.

### 4.8 Special values (fast checks)

$$X(e^{j0})=\sum_n x[n],\qquad X(e^{j\pi})=\sum_n(-1)^n x[n],\qquad x[0]=\frac{1}{2\pi}\int_{-\pi}^{\pi}X(e^{j\omega})d\omega$$

---

## 5. z-transform and ROC

`L2, L3, PS 3.21, 3.30, 3.37, 3.40`

### 5.1 What it is

A generalization of the Fourier transform that also works for unstable systems `L2`. A z-transform is **two things**: an algebraic expression and a region of convergence (the values of $\lvert z\rvert$ for which the sum converges). The properties on the sheet are properties of the algebraic expression; you supply the ROC.

### 5.2 ROC rules

1. The ROC depends only on $\lvert z\rvert$: a disk, a ring, or the outside of a circle, centered at the origin.
2. It contains no poles and is bounded by poles.
3. **Right-sided** signal (zero before some finite time): ROC is **outside the outermost pole** `L3`.
4. **Left-sided** signal: ROC is inside the innermost nonzero pole.
5. **Two-sided** signal: a ring between two poles.
6. **Finite-length** signal: the whole plane except possibly $z=0$ and/or $z=\infty$.
7. Sum or product of transforms: the ROC contains at least the intersection, and can be larger after a pole-zero cancellation.

### 5.3 Pairs to memorize (only $a^n u[n]$ is on the sheet)

| $x[n]$ | $X(z)$ | ROC |
|---|---|---|
| $\delta[n]$ | $1$ | all $z$ |
| $\delta[n-n_0]$ | $z^{-n_0}$ | all $z$ except $0$ (if $n_0>0$) or $\infty$ (if $n_0<0$) |
| $u[n]$ | $\dfrac{1}{1-z^{-1}}$ | $\lvert z\rvert>1$ |
| $-a^n u[-n-1]$ | $\dfrac{1}{1-az^{-1}}$ | $\lvert z\rvert<\lvert a\rvert$ |
| $u[-n-1]$ | $-\dfrac{1}{1-z^{-1}}$ | $\lvert z\rvert<1$ |
| $u[n]-u[n-N]$ ($N$-point box) | $\dfrac{1-z^{-N}}{1-z^{-1}}$ | $\lvert z\rvert>0$ |
| $a^n\big(u[n]-u[n-N]\big)$ | $\dfrac{1-a^Nz^{-N}}{1-az^{-1}}$ | $\lvert z\rvert>0$ |

The right-sided and left-sided exponentials have the **same algebraic expression**. Only the ROC (and a minus sign in time) tells them apart.

**A left-sided signal that does not match the table** `L3`: go back to the definition and use the infinite sum formula in powers of $z$. Lecture example:

$$g[n]=-\left(\tfrac12\right)^n u[-n+1]\quad\Longrightarrow\quad G(z)=-\tfrac12z^{-1}-\frac{1}{1-2z},\qquad\lvert z\rvert<\tfrac12$$

The condition for the sum to converge **is** the ROC ($\lvert 2z\rvert<1$ here). This ROC excludes the unit circle, so the DTFT does not converge.

### 5.4 Property missing from the sheet, and ROC effects

| Property | Sequence | Transform | ROC |
|---|---|---|---|
| Linearity | $\alpha_1x_1+\alpha_2x_2$ | $\alpha_1X_1+\alpha_2X_2$ | contains $R_1\cap R_2$ |
| Time shift | $x[n-n_0]$ | $z^{-n_0}X(z)$ | $R$, except possibly $0$ or $\infty$ |
| Time reversal | $x[-n]$ | $X(z^{-1})$ | $R$ inverted: $r_1<\lvert z\rvert<r_2$ becomes $\tfrac{1}{r_2}<\lvert z\rvert<\tfrac{1}{r_1}$ |
| Convolution | $x*h$ | $X(z)H(z)$ | contains $R_x\cap R_h$ |

### 5.5 Reading causality and stability off the ROC

| Property | z-domain test |
|---|---|
| Causal | ROC is outside the outermost pole **and** includes $z=\infty$ |
| Stable | ROC includes the unit circle |
| Causal and stable | every pole strictly inside the unit circle |

- Causal and stable are independent of each other.
- A pole at $\infty$ (numerator degree in $z$ larger than denominator degree, i.e. a leftover factor of $z$) means $h[n]$ has samples at negative $n$: **not causal** `PS 3.40`.
- A difference equation alone does not determine the ROC; you need "causal" or "stable" to pick it.

---

## 6. Poles and zeros

`L3, L4, L5, PS 3.37, 3.40`

### 6.1 Procedure

1. Multiply top and bottom by a power of $z$ so that $X(z)=\dfrac{N(z)}{D(z)}$ with polynomials **in $z$** (not $z^{-1}$).
2. Factor and cancel common factors.
3. Zeros are the roots of what is left on top, poles are the roots of what is left on the bottom.

$$\frac{1}{1-\tfrac12z^{-1}}=\frac{z}{z-\tfrac12}\qquad\text{zero at }0,\text{ pole at }\tfrac12$$

### 6.2 Two lecture notes to remember

- **Be careful about poles and zeros at $z=0$ and $z=\infty$.**
- **Number of poles equals number of zeros**, counting those at $0$ and $\infty$. If the degree in $z$ of the numerator is lower than the denominator by $k$, there are $k$ zeros at $\infty$; if higher by $k$, there are $k$ poles at $\infty$.

### 6.3 Roots of $z^N=c$

Write $c$ in polar form **with the $2\pi k$ included**, then take the root:

$$z^N=\lvert c\rvert e^{j(\angle c+2\pi k)}\quad\Longrightarrow\quad z=\lvert c\rvert^{1/N}\exp\!\left(j\frac{\angle c+2\pi k}{N}\right),\qquad k=0,1,\dots,N-1$$

| Equation | Roots | Source |
|---|---|---|
| $z^2=-\tfrac14$ | $\pm\tfrac{j}{2}$ | `L3` |
| $z^4=1$ | $1,j,-1,-j$ | `L3` |
| $z^4=\tfrac{1}{16}$ | $\tfrac12,\tfrac{j}{2},-\tfrac12,-\tfrac{j}{2}$ | `L4` |
| $z^2=-\tfrac{1}{16}$ | $\pm\tfrac{j}{4}$ | `L4, L5` |

### 6.4 Patterns from the lecture examples

- **$N$-point box:** $H(z)=\dfrac{z^N-1}{z^{N-1}(z-1)}$. $N$ zeros at the $N$-th roots of unity, the zero at $z=1$ **cancels** the pole at $z=1$, leaving $N-1$ zeros on the unit circle and $N-1$ poles at the origin. ROC $\lvert z\rvert>0$.
- **Truncated exponential** $h[n]=\left(\tfrac12\right)^n$ for $0\le n\le3$: $H(z)=\dfrac{z^4-\left(\tfrac12\right)^4}{z^3\left(z-\tfrac12\right)}$. Four zeros on the circle of radius $\tfrac12$, cancellation at $z=\tfrac12$, three poles at the origin.
- **A zero on the unit circle at angle $\omega_0$ means $H(e^{j\omega_0})=0$:** that frequency is removed.
- Real signals have complex poles and zeros in conjugate pairs.

### 6.5 Transforming a pole-zero plot

`PS 3.37`

| Operation | Effect on the plot |
|---|---|
| $x[n-n_0]$, factor $z^{-n_0}$ | adds $n_0$ poles at the origin (or cancels zeros there) |
| $x[-n]$, $z\to\tfrac1z$ | every pole and zero at $c$ moves to $\tfrac1c$ (radius inverted, angle negated); $0\leftrightarrow\infty$; ROC inverted |
| $x[-n+n_0]=x[-(n-n_0)]$ | flip first, then delay: $z^{-n_0}X(z^{-1})$ |

Count the zeros of $X(z)$ at $\infty$ before flipping: they land on the origin and cancel some of the new poles there. Time-domain cross-check: if $X(z)\sim z^{-m}$ as $z\to\infty$, then a causal $x[n]$ starts at $n=m$.

---

## 7. Filters: system function, difference equations, FIR and IIR

`L4, L5, PS 3.21, 3.30, 3.40`

### 7.1 The core chain

$$y=x*h\quad\Longrightarrow\quad Y(z)=X(z)H(z)\quad\Longrightarrow\quad H(z)=\frac{Y(z)}{X(z)}$$

- **Difference equation to $H(z)$:** take the z-transform of both sides ($x[n-k] \to z^{-k}X(z)$), collect, divide.
- **$H(z)$ to difference equation:** expand the denominator, cross-multiply, inverse transform term by term, solve for $y[n]$.
- **One input/output pair to $H(z)$** `PS 3.40`: $H=Y/X$, and choose the ROC of $H$ so that it is consistent with the ROCs of $X$ and $Y$.
- **Desired output to required input** `PS 3.30`: $X(z)=Y(z)/H(z)$. Poles and zeros swap roles.

### 7.2 What makes a filter practical

`L4` A filter is practical when the convolution can be carried out with a **finite number of arithmetic operations (multiplications and additions) per output sample**. Cost is measured in multiplications per output sample.

| Form | Multiplications per output sample | Stored coefficients |
|---|---|---|
| Non-recursive FIR, length $N$ | at most $N$ (and at most $N-1$ additions) | $b_k=h[k]$, $0\le k\le N-1$ |
| Recursive, orders $N$ and $M$ | at most $N+M+1$ | $a_1,\dots,a_N,b_0,\dots,b_M$ |

These are **upper bounds**: a coefficient equal to $1$ (or $0$) costs nothing. Lecture 5's example $y[n]=-\tfrac{1}{16}y[n-2]+x[n]-\tfrac12x[n-1]$ needs 2 multiplications.

### 7.3 FIR filters: the general results

For a non-recursive filter the stored coefficients **are** the impulse response: $b_k=h[k]$. Then, in a chain `L4`:

> all poles at the origin $\Rightarrow$ ROC is $\lvert z\rvert>0$ $\Rightarrow$ every FIR filter is **stable** $\Rightarrow$ its frequency response always converges.

### 7.4 An FIR filter can be written recursively

`L4` Use the finite sum formula to put $H(z)$ in closed form, then cross-multiply:

$$h[n]=\left\lbrace 1,\tfrac12,\tfrac14,\tfrac18\right\rbrace\quad\Longrightarrow\quad H(z)=\frac{1-\tfrac{1}{16}z^{-4}}{1-\tfrac12z^{-1}}\quad\Longrightarrow\quad y[n]=\tfrac12y[n-1]+x[n]-\tfrac{1}{16}x[n-4]$$

- Same filter as $y[n]=x[n]+\tfrac12x[n-1]+\tfrac14x[n-2]+\tfrac18x[n-3]$, but 2 multiplications instead of 3.
- The pole at $z=\tfrac12$ is cancelled by a zero, so the filter is still FIR. **A recursive difference equation does not imply IIR.**
- **There are infinitely many recursive difference equations for the same filter:** multiply $H(z)$ by any "fake pole-zero pair" $\dfrac{1-\alpha z^{-1}}{1-\alpha z^{-1}}$ and cross-multiply.

### 7.5 IIR filters

The sheet's definition: causal, rational $H(z)$, at least one pole **not at the origin** (after cancellation). Lecture example `L4, L5`:

$$H(z)=\frac{1-\tfrac12z^{-1}}{1+\tfrac{1}{16}z^{-2}}=\frac{z\left(z-\tfrac12\right)}{\left(z+\tfrac{j}{4}\right)\left(z-\tfrac{j}{4}\right)}$$

Zeros at $0$ and $\tfrac12$, poles at $\pm\tfrac{j}{4}$, ROC $\lvert z\rvert>\tfrac14$ (causal), stable, difference equation $y[n]=-\tfrac{1}{16}y[n-2]+x[n]-\tfrac12x[n-1]$.

### 7.6 Evaluating $H$ on the unit circle

Valid only if the ROC includes the unit circle.

| $\omega$ | $z$ | $z^{-1}$ | $z^{-2}$ | Meaning |
|---|---|---|---|---|
| $0$ | $1$ | $1$ | $1$ | DC gain $H(1)=\sum_n h[n]$ |
| $\pi/2$ | $j$ | $-j$ | $-1$ | |
| $\pi$ | $-1$ | $-1$ | $1$ | gain for $(-1)^n$ |

Example `PS 3.30`: $H(e^{j\pi/2})=\dfrac{1+j}{1.25}=0.8\sqrt2e^{j\pi/4}$, so $\cos(0.5\pi n)\to0.8\sqrt2\cos(0.5\pi n+\tfrac{\pi}{4})$.

### 7.7 Output for a given input

$$Y(z)=H(z)X(z),\qquad\text{ROC}_Y\supseteq\text{ROC}_H\cap\text{ROC}_X$$

- **Cancellation:** a zero of $H$ at an input pole removes that mode. $H$ with a zero at $z=1$ cancels the pole of $u[n]$, so the step response decays to zero `PS 3.30`.
- **Left-sided input into a causal system** `PS 3.21`: the ROC is a ring, and the output is two-sided. System poles give right-sided terms; the input pole gives a left-sided term.

### 7.8 Filter design ideas

`L5`

- The goal is a desired frequency-response behavior at **low cost** in multiplications per output sample.
- For real $h[n]$ the magnitude is even, so specifications are drawn on $0\le\omega\le\pi$. The frequency $\omega=\pi$ corresponds to half the analog sampling frequency.
- Two magnitude behaviors are costly: **flatness** over a band and **steepness** of the transitions.
- If flatness and steepness are extreme (an ideal brick-wall response), they **cannot be achieved by a filter with a rational system function**.

---

## 8. Getting the impulse response

`L5, PS 3.21, 3.30, 3.40`

### 8.1 Way 1: run the difference equation with $x[n]=\delta[n]$

$$h[n]=\sum_{k=1}^{N}a_kh[n-k]+\sum_{m=0}^{M}b_m\delta[n-m],\qquad h[n]=0\text{ for }n<0$$

$$h[0]=b_0,\qquad h[1]=a_1b_0+b_1,\qquad\dots$$

Good for the first few samples and for checking a closed form.

### 8.2 Way 2: partial fraction expansion

$$H(z)=\frac{N(z)}{D(z)}=\sum_{k=1}^{N}\frac{A_k}{1-p_kz^{-1}},\qquad A_k=\left(1-p_kz^{-1}\right)H(z)\Big\rvert_{z=p_k}$$

where the $p_k$ are the nonzero poles. For a causal filter $h[n]=\sum_kA_kp_k^nu[n]$: **a linear combination of exponentials** (decaying if the filter is stable).

Details the lecture formula leaves out:

1. **Divide first when the numerator order in $z^{-1}$ is at least the denominator order.** The quotient is a polynomial in $z^{-1}$ and inverts to impulses. With equal orders the constant is the ratio of the highest-order coefficients. `PS 3.21`: $\dfrac{-0.5}{-0.125}=4$, giving $h[n]=4\delta[n]-(0.25)^nu[n]+(-0.5)^nu[n]$.
2. **Each term is inverted according to the ROC:**

| Pole $p_k$ relative to the ROC | $\dfrac{A_k}{1-p_kz^{-1}}$ inverts to |
|---|---|
| ROC lies outside $\lvert p_k\rvert$ | $A_kp_k^nu[n]$ |
| ROC lies inside $\lvert p_k\rvert$ | $-A_kp_k^nu[-n-1]$ |

3. **Complex-conjugate poles** give conjugate residues, and the pair combines to a real sequence: $A p^n+A^*(p^*)^n=2\lvert A\rvert\lvert p\rvert^n\cos(n\angle p+\angle A)$.
4. **Repeated poles are not needed for EC516 exams** (stated in Lecture 5).

### 8.3 Worked example: the Lecture 5 IIR filter

Poles $p_{1,2}=\pm\tfrac{j}{4}$.

$$A_1=\frac{1-\tfrac12z^{-1}}{1+\tfrac{j}{4}z^{-1}}\Bigg\rvert_{z=j/4}=\frac{1+2j}{2}=\tfrac12+j,\qquad A_2=A_1^*=\tfrac12-j$$

$$h[n]=\left(\tfrac14\right)^n\Big[\cos\!\left(\tfrac{\pi n}{2}\right)-2\sin\!\left(\tfrac{\pi n}{2}\right)\Big]u[n]=\left\{1,-\tfrac12,-\tfrac{1}{16},\tfrac{1}{32},\dots\right\}$$

Check with Way 1: $h[0]=1$, $h[1]=-\tfrac12$, $h[2]=-\tfrac{1}{16}h[0]=-\tfrac{1}{16}$, $h[3]=-\tfrac{1}{16}h[1]=\tfrac{1}{32}$.

### 8.4 Other inversion tools

- A finite polynomial in $z^{\pm1}$ inverts by inspection: $1-0.25z^{-2}\to\delta[n]-0.25\delta[n-2]$.
- A leading factor of $z$ is a one-sample **advance**.
- Rewrite into a tabulated shape before transforming `PS 3.40`: $\left(\tfrac12\right)^{n-1}u[n+1]=4\left(\tfrac12\right)^{n+1}u[n+1]$, which is $4a^mu[m]$ advanced by one.

---

## 9. Phase: envelope, fractional delay, phase delay, linear phase, group delay

`L6, L7, L8` Lecture 6 is complete; Lectures 7 and 8 are only partly available (see the note at the top).

### 9.1 Continuous-time envelope

A discrete-time signal with a Fourier transform has a **unique** continuous-time counterpart, its envelope $x_e(t)$. The relationship is invertible.

- **DT to CT:** extract the one period of $X(e^{j\omega})$ from $-\pi$ to $\pi$ and call it $X_e(j\omega)$ (zero outside).
- **CT to DT:** replicate $X_e(j\omega)$ every $2\pi$; in time, sample $x_e(t)$ at every integer.
- The envelope is the unique CT signal which, when sampled at the integers, gives $x[n]$ with **no aliasing**.

| $x[n]$ | $x_e(t)$ |
|---|---|
| $\delta[n]$ | $\dfrac{\sin\pi t}{\pi t}$ |
| $e^{j\omega_0n}$ | $e^{j\omega_0t}$ |

### 9.2 Fractional delay

- **Shift:** $x[n-n_0]$ with $n_0$ an integer.
- **Delay:** $x_{t_0}[n]$ with $t_0$ any real number, defined by: delay the envelope, then read it at integer times.

$$x_{t_0}[n]=x_e(t-t_0)\Big\rvert_{t=n}$$

**What is guaranteed** for $-\pi\le\omega\le\pi$:

$$\lvert X_{t_0}(e^{j\omega})\rvert=\lvert X(e^{j\omega})\rvert,\qquad\angle X_{t_0}(e^{j\omega})=\angle X(e^{j\omega})-t_0\omega$$

The magnitude is unchanged, and a term linear in frequency is subtracted from the phase. Equivalently, a delay of $t_0$ is the filter $H(e^{j\omega})=e^{-j\omega t_0}$ on $\lvert\omega\rvert\le\pi$.

**Two examples from lecture:**

$$\delta[n]\text{ delayed by }\tfrac12:\qquad x_{1/2}[n]=\frac{\sin\pi(n-\tfrac12)}{\pi(n-\tfrac12)}=\frac{(-1)^{n+1}}{\pi(n-\tfrac12)}$$

with $x_{1/2}[0]=x_{1/2}[1]=\tfrac{2}{\pi}$ and $x_{1/2}[-1]=x_{1/2}[2]=-\tfrac{2}{3\pi}$. A half-sample delay turns a single impulse into an infinitely long sequence.

$$e^{j\omega_0n}\text{ delayed by }\tfrac12:\qquad x_{1/2}[n]=e^{j\omega_0(n-\frac12)}$$

A complex exponential stays a complex exponential at the same frequency; only a constant phase factor appears.

> **Picture it (guitar recording).** Two mics on one guitar, one a few centimeters farther from the soundboard. The extra path is almost never a whole number of samples at 48 kHz. Time-aligning the two tracks means delaying one by something like 0.37 samples: the DAW rebuilds the smooth waveform between the samples (the envelope), slides it, and reads off new sample values.

### 9.3 Phase delay

$$\tau_{ph}(\omega)=-\frac{\angle H(e^{j\omega})}{\omega}$$

This is the (fractional) delay the filter applies to the component at frequency $\omega$, from the gain-and-delay form in Section 3.2.

### 9.4 Linear phase

A filter has **linear phase** when $\angle H(e^{j\omega})=(\text{constant})\cdot\omega$, i.e. $H(e^{j\omega})=\lvert H(e^{j\omega})\rvert e^{-j\alpha\omega}$. Then $\tau_{ph}(\omega)=\alpha$ for every $\omega$:

> **A linear-phase filter delays all frequency components identically.**

So the filter changes the gains of the components but does not move them relative to each other in time.

> **Picture it (guitar recording).** A pluck is a sharp attack made of many harmonics that all start together. Run it through a linear-phase EQ and every harmonic arrives late by the same amount, so the attack keeps its shape. With a non-linear phase the harmonics arrive at slightly different times and the transient smears.

### 9.5 Group delay

`L8, first photo` In some applications we want $\tau_{ph}(\omega)$ constant, and only a limited class of filters can achieve that. The quantity boxed on the board:

$$\tau_g(\omega)=-\frac{d}{d\omega}\angle H(e^{j\omega})$$

How the two delays relate:

| Phase | $\tau_{ph}(\omega)$ | $\tau_g(\omega)$ |
|---|---|---|
| $-\alpha\omega$ (linear) | $\alpha$ | $\alpha$ |
| $\beta-\alpha\omega$ (linear plus a constant) | $\alpha-\dfrac{\beta}{\omega}$, not constant | $\alpha$, constant |

Problem-set examples you can now reinterpret:

- `PS 2.62` $\angle H=-\tfrac{\omega}{2}-\tfrac{\pi}{4}$: group delay $\tfrac12$ sample at every frequency; the output was the input **delayed by half a sample** and multiplied by $e^{-j\pi/4}$.
- `PS 2.41` $\angle H=-\tfrac{\omega}{3}-\tfrac{\pi}{2}$ for $\omega>0$: $\tau_g=\tfrac13$, while $\tau_{ph}\!\left(\tfrac{\pi}{2}\right)=\dfrac{2\pi/3}{\pi/2}=\tfrac43$.
- The $N$-point box has phase $-\omega\tfrac{N-1}{2}$ (plus jumps of $\pi$ at its nulls): delay $\tfrac{N-1}{2}$, a half-integer when $N$ is even.

> **Likely in the missing photos; check against your notes.** The standard continuation of "limited class of filters that can achieve this" is that an FIR filter with a **symmetric** impulse response, $h[n]=h[N-1-n]$, has linear phase with delay $\tfrac{N-1}{2}$ (antisymmetric $h[n]=-h[N-1-n]$ gives the same delay plus a constant $\tfrac{\pi}{2}$). I could not confirm this was on the board, so treat it as a pointer rather than as confirmed exam material.

---

## 10. Algebra you are expected to do fast

**Complex numbers**

$$e^{j\pi/2}=j,\qquad e^{j\pi}=-1,\qquad e^{-j\pi/2}=-j,\qquad\frac1j=-j,\qquad e^{j2\pi k}=1$$

$$a+jb=\sqrt{a^2+b^2}e^{j\theta},\quad\theta=\mathrm{atan2}(b,a)\qquad\text{(check the quadrant)}$$

$$\left\lvert\frac{z_1}{z_2}\right\rvert=\frac{\lvert z_1\rvert}{\lvert z_2\rvert},\qquad\angle\frac{z_1}{z_2}=\angle z_1-\angle z_2,\qquad z+z^*=2\mathrm{Re}\{z\}$$

**Trigonometry**

$$\cos(-\theta)=\cos\theta,\qquad\sin(-\theta)=-\sin\theta,\qquad\sin\theta=\cos\!\left(\theta-\tfrac{\pi}{2}\right)$$

$$A\cos\theta+B\sin\theta=\sqrt{A^2+B^2}\cos\!\big(\theta-\operatorname{atan2}(B,A)\big)$$

$$\cos(\pi n)=(-1)^n,\qquad\sin(\pi n)=0,\qquad\sin\!\big(\pi(n-\tfrac12)\big)=(-1)^{n+1}$$

| $\theta$ | $0$ | $\pi/6$ | $\pi/4$ | $\pi/3$ | $\pi/2$ |
|---|---|---|---|---|---|
| $\cos\theta$ | $1$ | $\tfrac{\sqrt3}{2}$ | $\tfrac{\sqrt2}{2}$ | $\tfrac12$ | $0$ |
| $\sin\theta$ | $0$ | $\tfrac12$ | $\tfrac{\sqrt2}{2}$ | $\tfrac{\sqrt3}{2}$ | $1$ |

**Geometric sums beyond the sheet's two**

$$\sum_{k=N_1}^{N_2}\alpha^k=\frac{\alpha^{N_1}-\alpha^{N_2+1}}{1-\alpha},\qquad\sum_{k=N_1}^{\infty}\alpha^k=\frac{\alpha^{N_1}}{1-\alpha}\quad(\lvert\alpha\rvert<1)$$

For a sum running to $-\infty$, substitute $m=-n$ first so it becomes a sum in positive powers (this is what Lecture 3 did for the left-sided example).

---

## 11. Recipes by question type

**A. Sinusoid through a system given $H(e^{j\omega})$**

1. Reduce $\omega_0$ into $(-\pi,\pi]$; fix the phase sign if it went negative.
2. Is $H$ conjugate-symmetric (real $h$)? If yes, use gain and phase shift. If no, split with Euler and treat $\pm\omega_0$ separately.
3. If the input has several terms, do each separately and add. Impulses and steps go through $h[n]$ in the time domain.

**B. Everything from a given $H(z)$**

1. Polynomials in $z$, factor, cancel: poles and zeros, including $0$ and $\infty$.
2. ROC from "causal" / "stable" / the given ROC.
3. Stability: is the unit circle in the ROC?
4. Difference equation: cross-multiply. Count multiplications if asked.
5. $h[n]$: divide if needed, partial fractions, invert by ROC. Check $h[0]$ and $h[1]$ by recursion.
6. Frequency response at a specific $\omega$: substitute $z=e^{j\omega}$ (table in 7.6).

**C. FIR filter given $h[n]$**

1. Non-recursive equation: $y[n]=\sum_kh[k]x[n-k]$.
2. $H(z)$ as a polynomial in $z^{-1}$; closed form with the finite sum formula if $h$ is geometric.
3. Zeros from the numerator, poles at the origin, ROC $\lvert z\rvert>0$, stable.
4. Recursive version by cross-multiplying the closed form; compare multiplication counts.

**D. Output for a given input, via z**

1. $X(z)$ with ROC; $Y=HX$; cancel; ROC is the intersection (or larger after cancellation).
2. Partial fractions; assign each pole to right-sided or left-sided by the ROC.

**E. Delay questions**

1. Phase delay at $\omega_0$: $-\angle H(e^{j\omega_0})/\omega_0$. Group delay: minus the slope of the phase.
2. Delaying a specific signal by $t_0$: multiply its DTFT by $e^{-j\omega t_0}$ on $\lvert\omega\rvert\le\pi$, or delay the envelope and sample at integers.

**F. Finite convolution**

1. Predict the support and length.
2. Stack scaled, shifted copies of $h$ (or use linearity on a response you already have).
3. Check $\sum y=\sum x\sum h$.

---

## 12. Traps

1. Reading $H(e^{j\omega})$ at a frequency outside $(-\pi,\pi]$ without reducing it.
2. Dropping the phase sign flip when the reduced frequency is negative.
3. Using the cosine shortcut when the phase is not odd.
4. Shifts add under convolution: $x[n-n_0]*h[n-n_0]=y[n-2n_0]$.
5. Multiplying by $\delta[n-n_0]$ samples; convolving with it shifts.
6. Reading feedback coefficients with the wrong sign (Section 0, item 2).
7. Calling a recursive difference equation IIR without checking for pole-zero cancellation.
8. Forgetting poles and zeros at $0$ and $\infty$; the pole and zero counts must match.
9. Taking a root without the $2\pi k$: $z^N=c$ has $N$ roots.
10. Saying a pole is "in" the ROC. It never is; the ROC lies outside or inside the pole's radius.
11. Left-sided inversion needs **both** the minus sign and $u[-n-1]$.
12. Skipping the division step in partial fractions and losing the $\delta[n]$ terms.
13. Evaluating $H(e^{j\omega})$ when the unit circle is not in the ROC.
14. Treating $\dfrac{\sin(\omega N/2)}{\sin(\omega/2)}$ as a magnitude; it changes sign.
15. The $0/0$ sample of a (shifted) sinc.
16. A fractional delay of an impulse is not an impulse.
17. Phase delay and group delay coincide only when the phase is a pure multiple of $\omega$.
18. Causal does not imply stable, and stable does not imply causal.

---

## 13. Coverage map and errata

### 13.1 Lectures

| Lecture | Date | Topics | Sections |
|---|---|---|---|
| 1 | Sept 2 | Convolution vs multiplication, shift rule, derivation, mechanics, LTI preserves frequency | 2, 3.1 |
| 2 | Sept 9 | Gain and delay, DTFT and properties, box DTFT, z-transform and ROC | 3.2, 4, 5.1 |
| 3 | Sept 14 | Left-sided example, z properties and pairs, rational transforms, poles and zeros, right-sided ROC rule, convergence and stability | 4.3, 5, 6 |
| 4 | Sept 16 | Practical filters, causality assumption, FIR results, recursive form of FIR, fake pole-zero pairs | 7.1 to 7.4 |
| 5 | Sept 21 | IIR example, operation counts, impulse response two ways, filter design ideas | 7.5, 7.8, 8 |
| 6 | Sept 23 | Envelope, fractional delay, phase delay, linear phase | 9.1 to 9.4 |
| 7 | Sept 28 | Linear phase, phase delay (first photo only; 4 photos missing) | 9.3, 9.4 |
| 8 | Sept 30 | Constant phase delay, group delay (first photo only; about 15 photos missing) | 9.5 |

### 13.2 Problem sets

| Problem | Ideas | Sections |
|---|---|---|
| 2.21 | Shift, flip, downsample, mask with a step, sample with an impulse | 1 |
| 2.29 | Step response as a running sum; linearity and time invariance | 2.3, 2.4 |
| 2.30 | Cascade, box with box, null of a moving sum | 2.3, 2.4, 3.6 |
| 2.62 | Frequency reduction, non-symmetric $H$, half-sample delay | 1, 3.3, 9.5 |
| 2.32 | Real, imaginary, magnitude, phase of a first-order term | 4.2, 4.6 |
| 2.37 | Cascade in frequency, bandpass, delay, inverse DTFT | 3.4, 3.5, 4.7 |
| 2.41 | Frequency reduction with sign flip, all-pass phase | 1, 3.3, 9.5 |
| 2.45 | First difference, passband and stopband, term-by-term analysis | 3.5, 3.6 |
| 3.21 | ROC, stability, difference equation, division plus partial fractions, two-sided output | 5, 7.7, 8.2 |
| 3.30 | Cancellation, inverse system, $H$ on the unit circle | 7.1, 7.6, 7.7 |
| 3.37 | Time reversal and shift of a pole-zero plot | 6.5 |
| 3.40 | $H=Y/X$, rewriting into a standard pair, noncausal system | 5.5, 7.1, 8.4 |

### 13.3 Points where the source documents need care

- **Formula sheet:** the $e^{j\omega_0n}$ pair as printed (Section 0, item 1).
- **Lecture 2:** the multiplication property was written without the factor $\dfrac{1}{2\pi}$ in front of the integral; the factor belongs there (Section 4.1).
- **PS 3.37 posted solution:** it reports a triple pole of $Y(z)$ at $z=0$. With the pole-zero pattern stated in that solution (one zero at the origin, three poles), $X(z)$ has two zeros at $\infty$; after $z\to\tfrac1z$ they sit at the origin and cancel two of the three poles added by $z^{-3}$. Only a **simple** pole remains at $z=0$. Cross-check: $x[n]$ starts at $n=2$, so $y[n]=x[3-n]$ ends at $n=1$, and its highest negative power is $z^{-1}$.
- **PS 3.40 posted solution:** the ROC is given as $\lvert z\rvert>0.5$; because of the pole at $\infty$ it is strictly $0.5<\lvert z\rvert<\infty$.
- **PS 3.21(f) posted solution:** the phrase "poles are inside the ROC" means the poles lie inside the inner boundary of the ring, which is why those terms are right-sided.

### 13.4 Low priority

These do not appear anywhere in the lectures I could read or in PS1–3: repeated poles (explicitly excluded in Lecture 5), Parseval's relation, the differentiation properties, the DTFT of $u[n]$.
