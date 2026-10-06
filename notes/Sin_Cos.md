# Sin and Cos Algebra Sheet

Notation: $j = \sqrt{-1}$, angles in radians. $A$, $B$, $x$ are any real angles.

---

## 1. Basics

$$
\sin^2 x + \cos^2 x = 1
$$

| Property | Cosine | Sine |
|---|---|---|
| Even / odd | $\cos(-x) = \cos x$ | $\sin(-x) = -\sin x$ |
| Period $2\pi$ | $\cos(x + 2\pi) = \cos x$ | $\sin(x + 2\pi) = \sin x$ |
| Shift by $\pi$ | $\cos(x + \pi) = -\cos x$ | $\sin(x + \pi) = -\sin x$ |
| Shift by $+\tfrac{\pi}{2}$ | $\cos(x + \tfrac{\pi}{2}) = -\sin x$ | $\sin(x + \tfrac{\pi}{2}) = \cos x$ |
| Shift by $-\tfrac{\pi}{2}$ | $\cos(x - \tfrac{\pi}{2}) = \sin x$ | $\sin(x - \tfrac{\pi}{2}) = -\cos x$ |
| Reflect about $\tfrac{\pi}{2}$ | $\cos(\pi - x) = -\cos x$ | $\sin(\pi - x) = \sin x$ |
| Complement | $\cos(\tfrac{\pi}{2} - x) = \sin x$ | $\sin(\tfrac{\pi}{2} - x) = \cos x$ |

---

## 2. Angle sum and difference

$$
\begin{aligned}
\cos(A + B) &= \cos A\cos B - \sin A\sin B \\
\cos(A - B) &= \cos A\cos B + \sin A\sin B \\
\sin(A + B) &= \sin A\cos B + \cos A\sin B \\
\sin(A - B) &= \sin A\cos B - \cos A\sin B
\end{aligned}
$$

---

## 3. Double angle

$$
\begin{aligned}
\sin 2x &= 2\sin x\cos x \\
\cos 2x &= \cos^2 x - \sin^2 x \\
        &= 2\cos^2 x - 1 \\
        &= 1 - 2\sin^2 x
\end{aligned}
$$

---

## 4. Power reduction and half angle

$$
\begin{aligned}
\cos^2 x &= \tfrac{1}{2}\left(1 + \cos 2x\right) \\
\sin^2 x &= \tfrac{1}{2}\left(1 - \cos 2x\right) \\
\sin x\cos x &= \tfrac{1}{2}\sin 2x
\end{aligned}
$$

$$
\cos\tfrac{x}{2} = \pm\sqrt{\tfrac{1 + \cos x}{2}}, \qquad
\sin\tfrac{x}{2} = \pm\sqrt{\tfrac{1 - \cos x}{2}}
$$

(The sign depends on which quadrant $\tfrac{x}{2}$ is in.)

---

## 5. Product to sum

$$
\begin{aligned}
\cos A\cos B &= \tfrac{1}{2}\left[\cos(A - B) + \cos(A + B)\right] \\
\sin A\sin B &= \tfrac{1}{2}\left[\cos(A - B) - \cos(A + B)\right] \\
\sin A\cos B &= \tfrac{1}{2}\left[\sin(A + B) + \sin(A - B)\right] \\
\cos A\sin B &= \tfrac{1}{2}\left[\sin(A + B) - \sin(A - B)\right]
\end{aligned}
$$

---

## 6. Sum to product

$$
\begin{aligned}
\cos A + \cos B &= 2\cos\!\left(\tfrac{A+B}{2}\right)\cos\!\left(\tfrac{A-B}{2}\right) \\
\cos A - \cos B &= -2\sin\!\left(\tfrac{A+B}{2}\right)\sin\!\left(\tfrac{A-B}{2}\right) \\
\sin A + \sin B &= 2\sin\!\left(\tfrac{A+B}{2}\right)\cos\!\left(\tfrac{A-B}{2}\right) \\
\sin A - \sin B &= 2\cos\!\left(\tfrac{A+B}{2}\right)\sin\!\left(\tfrac{A-B}{2}\right)
\end{aligned}
$$

How to remember them: write $A = M + D$ and $B = M - D$, where $M = \tfrac{A+B}{2}$ is the average and $D = \tfrac{A-B}{2}$ is the half-difference, then expand with the angle-sum formulas from section 2. Half the terms cancel.

---

## 7. Euler's formula

$$
\begin{aligned}
e^{jx} &= \cos x + j\sin x \\
e^{-jx} &= \cos x - j\sin x
\end{aligned}
$$

$$
\cos x = \frac{e^{jx} + e^{-jx}}{2}, \qquad
\sin x = \frac{e^{jx} - e^{-jx}}{2j}
$$

Useful facts:

$$
\left|e^{jx}\right| = 1, \qquad
e^{jA}\,e^{jB} = e^{j(A+B)}, \qquad
\left(e^{jx}\right)^{*} = e^{-jx}
$$

| $x$ | $0$ | $\tfrac{\pi}{2}$ | $\pi$ | $-\tfrac{\pi}{2}$ | $2\pi k$ |
|---|---|---|---|---|---|
| $e^{jx}$ | $1$ | $j$ | $-1$ | $-j$ | $1$ |

---

## 8. Adding two complex exponentials

Factor out the average phase; what is left is a cosine or a sine.

$$
\begin{aligned}
e^{jA} + e^{jB} &= 2\,e^{j\frac{A+B}{2}}\cos\!\left(\tfrac{A-B}{2}\right) \\
e^{jA} - e^{jB} &= 2j\,e^{j\frac{A+B}{2}}\sin\!\left(\tfrac{A-B}{2}\right) \\
e^{jA} + e^{-jB} &= 2\,e^{j\frac{A-B}{2}}\cos\!\left(\tfrac{A+B}{2}\right) \\
e^{jA} - e^{-jB} &= 2j\,e^{j\frac{A-B}{2}}\sin\!\left(\tfrac{A+B}{2}\right)
\end{aligned}
$$

Special cases that show up constantly in DSP:

$$
\begin{aligned}
1 + e^{jx} &= 2\,e^{jx/2}\cos\tfrac{x}{2} \\
1 - e^{jx} &= -2j\,e^{jx/2}\sin\tfrac{x}{2}
\end{aligned}
$$

---

## 9. Combining sin and cos at the same frequency

$$
a\cos x + b\sin x = R\cos(x - \varphi), \qquad
R = \sqrt{a^2 + b^2}, \quad \varphi = \operatorname{atan2}(b,\, a)
$$

Two cosines with the same frequency but different amplitude and phase add as phasors:

$$
A_1\cos(\omega n + \varphi_1) + A_2\cos(\omega n + \varphi_2)
= \operatorname{Re}\!\left\{\left(A_1 e^{j\varphi_1} + A_2 e^{j\varphi_2}\right)e^{j\omega n}\right\}
$$

---

## 10. Common values

| $x$ | $0$ | $\tfrac{\pi}{6}$ | $\tfrac{\pi}{4}$ | $\tfrac{\pi}{3}$ | $\tfrac{\pi}{2}$ | $\pi$ | $\tfrac{3\pi}{2}$ |
|---|---|---|---|---|---|---|---|
| $\sin x$ | $0$ | $\tfrac{1}{2}$ | $\tfrac{\sqrt{2}}{2}$ | $\tfrac{\sqrt{3}}{2}$ | $1$ | $0$ | $-1$ |
| $\cos x$ | $1$ | $\tfrac{\sqrt{3}}{2}$ | $\tfrac{\sqrt{2}}{2}$ | $\tfrac{1}{2}$ | $0$ | $-1$ | $0$ |

---

## 11. Derivatives and integrals

$a$, $b$, $\omega$ are constants with $a \neq 0$, $\omega \neq 0$; $C$ is the constant of integration.

| $f(x)$ | $\dfrac{d}{dx}f(x)$ | $\displaystyle\int f(x)\,dx$ |
|---|---|---|
| $\sin x$ | $\cos x$ | $-\cos x + C$ |
| $\cos x$ | $-\sin x$ | $\sin x + C$ |
| $e^{x}$ | $e^{x}$ | $e^{x} + C$ |
| $\sin(ax + b)$ | $a\cos(ax + b)$ | $-\tfrac{1}{a}\cos(ax + b) + C$ |
| $\cos(ax + b)$ | $-a\sin(ax + b)$ | $\tfrac{1}{a}\sin(ax + b) + C$ |
| $e^{ax}$ | $a\,e^{ax}$ | $\tfrac{1}{a}e^{ax} + C$ |
| $e^{j\omega x}$ | $j\omega\,e^{j\omega x}$ | $\tfrac{1}{j\omega}e^{j\omega x} + C$ |

Patterns worth remembering:

$$
\frac{d}{dx}\cos x = \cos\!\left(x + \tfrac{\pi}{2}\right), \qquad
\frac{d}{dx}\sin x = \sin\!\left(x + \tfrac{\pi}{2}\right)
$$

Differentiating a sinusoid advances its phase by $\tfrac{\pi}{2}$; integrating delays it by $\tfrac{\pi}{2}$. For $e^{j\omega x}$, differentiating multiplies by $j\omega$ and integrating divides by $j\omega$.

Squares and products:

$$
\begin{aligned}
\int \sin^2 x\,dx &= \tfrac{x}{2} - \tfrac{1}{4}\sin 2x + C \\
\int \cos^2 x\,dx &= \tfrac{x}{2} + \tfrac{1}{4}\sin 2x + C \\
\int \sin x\cos x\,dx &= \tfrac{1}{2}\sin^2 x + C
\end{aligned}
$$

Exponential times sinusoid:

$$
\begin{aligned}
\int e^{ax}\cos bx\,dx &= \frac{e^{ax}\left(a\cos bx + b\sin bx\right)}{a^2 + b^2} + C \\
\int e^{ax}\sin bx\,dx &= \frac{e^{ax}\left(a\sin bx - b\cos bx\right)}{a^2 + b^2} + C
\end{aligned}
$$

Over one full period (the basis of Fourier series), for integers $k$, $m$:

$$
\int_{0}^{2\pi} e^{jkx}\,dx =
\begin{cases}
2\pi, & k = 0 \\
0, & k \neq 0
\end{cases}
\qquad\Longrightarrow\qquad
\int_{0}^{2\pi} e^{jkx}\,e^{-jmx}\,dx =
\begin{cases}
2\pi, & k = m \\
0, & k \neq m
\end{cases}
$$

$$
\int_{0}^{2\pi}\sin x\,dx = \int_{0}^{2\pi}\cos x\,dx = 0, \qquad
\int_{0}^{2\pi}\sin^2 x\,dx = \int_{0}^{2\pi}\cos^2 x\,dx = \pi
$$

---

## Worked example

Simplify $y[n] = \tfrac{1}{2}e^{j(\pi n/4 - \pi/24)} + \tfrac{1}{2}e^{-j(\pi n/4 + 11\pi/24)}$.

Let $A = \tfrac{\pi n}{4} - \tfrac{\pi}{24}$ and $B = \tfrac{\pi n}{4} + \tfrac{11\pi}{24}$. Then

$$
\tfrac{A+B}{2} = \tfrac{\pi n}{4} + \tfrac{5\pi}{24}, \qquad
\tfrac{A-B}{2} = -\tfrac{\pi}{4}
$$

Using the third line of section 8:

$$
y[n] = \tfrac{1}{2}\left(e^{jA} + e^{-jB}\right)
= e^{j\frac{A-B}{2}}\cos\!\left(\tfrac{A+B}{2}\right)
= e^{-j\pi/4}\cos\!\left(\tfrac{\pi n}{4} + \tfrac{5\pi}{24}\right)
$$
