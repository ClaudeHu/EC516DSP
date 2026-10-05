# Lecture 3

## z transform

**Properties**

1. Linearity: $a\,x_1[n] + b\,x_2[n] \;\longleftrightarrow\; a\,X_1(z) + b\,X_2(z)$
2. Shift: $x[n-k] \;\longleftrightarrow\; z^{-k} X(z)$
3. Flip: $x[-n] \;\longleftrightarrow\; X(z^{-1})$
4. Conjugation: $x^{\ast}[n] \;\longleftrightarrow\; X^{\ast}(z^{\ast})$
5. Convolution: $x_1[n] * x_2[n] \;\longleftrightarrow\; X_1(z)\, X_2(z)$

### Rational z-transform

$X(z) = \frac{N(z)}{D(z)}$, where N(z) and D(z) are polynomials in z

cancel common factor: $X(z) = \frac{N\`(z)}{D\`(z)}$

* zeros: solution to $N\`(z) = 0$
* poles: solution to ${D\`(z)} = 0$
