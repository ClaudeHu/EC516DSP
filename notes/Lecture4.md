# Lecture 3

## Digital Filter (LTI)

**Terminology**

* $h[n]$: impulse response
* $H(e^{j\omega})$: frequency response
* $H(z)$: systsem function (transfer function)


**Practical Filter**

Convolution can be performed via finite number of arithmetric operation per output sample

### Calsaulity

* h[n] = 0 for n < 0

### FIR (finite impulse response) Filter

h[n] = 0 for n > N - 1

$$
y[n] = \sum_{k=0}^{N-1}h[k]x[n-k]
$$

$$
\beta_{k} = h[k], 0 \leq k \leq N-1
$$

$$
y[n] = \sum_{k=0}^{N-1}\beta_{k}x[n-k]
$$
