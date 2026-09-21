# EC516DSP

## Lecture 1

### Convolution sum

$$
y[n] = x[n] * h[n] = \sum_{k=-\infty}^{\infty} x[k]h[n-k]
$$

#### Example

The input x[n] is the guitarist's plucks: a strong pluck at n = 0, silence at n = 1, and a softer pluck at n = 2.

$$
x[n] = \{\underset{\uparrow}{1},\ 0,\ 0.5\}
$$

The impulse response h[n] is the hall's echoes: direct sound (1), an early wall reflection (0.6), and a fainter late reflection (0.3).

$$
h[n] = \{\underset{\uparrow}{1},\ 0.6,\ 0.3\}
$$

Convolution sum:

$$
\begin{aligned}
y[0] &= x[0]h[0] = (1)(1) = 1 \\
y[1] &= x[0]h[1] + x[1]h[0] = (1)(0.6) + (0)(1) = 0.6 \\
y[2] &= x[0]h[2] + x[1]h[1] + x[2]h[0] = 0.3 + 0 + 0.5 = 0.8 \\
y[3] &= x[1]h[2] + x[2]h[1] = 0 + (0.5)(0.6) = 0.3 \\
y[4] &= x[2]h[2] = (0.5)(0.3) = 0.15
\end{aligned}
$$
