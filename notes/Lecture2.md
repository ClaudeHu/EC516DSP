# Lecture 2

## Frequency Domain

$$
e^{j\omega_0 n} = cos(\omega_0 n) + j sin(\omega_0 n)
$$
$$
cos(\omega_0 n) = \frac{1}{2}e^{j\omega_0 n} + \frac{1}{2}e^{-j\omega_0 n}
$$
$$
sin(\omega_0 n) = \frac{j}{2}e^{-j\omega_0 n} - \frac{j}{2}e^{j\omega_0 n}
$$

### Gain & Delay

$$
Frequency\ Response:\ H(e^{j\omega}) = |H(e^{j\omega})| e^{j\angle H(e^{j\omega}}
$$
$$
Gain = Magnitude = |H(e^{j\omega})|
$$
$$
y[n] = H(e^{j\omega}) e^{j\omega n} = |H(e^{j\omega})| e^{j\angle H(e^{j\omega})}  e^{j\omega n} = |H(e^{j\omega})| e^{j\omega n + j\angle H(e^{j\omega})} = \underbrace{|H(e^{j\omega})|}_{gain} e^{j\omega (n - \underbrace{\frac{\angle H(e^{j\omega})}{\omega})}_{delay}} 
$$


**Example**

Zenith watch movement  beats at 28,800 vibrations per hour = $4 Hz$ oscillation
Discrete time resonator model: 
