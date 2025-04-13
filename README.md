# Shor's Algorithm

## Steps:
### Classical part
1. Select a number $N=p*q$, where $p$ and $q$ are prime numbers that should be determined using the algorithm.
2. Pick a number $a$ so that $1< a < N$.
3. Calculate the greatest common divisor of $a$ and $N$ ($K=gcd(a,N)$).
4. If $K\neq 1$ then $p=K$ is a non-trivial factor of $N$ and the other factor would be $q=N/K$, and the algorithm ends. If $K=1$, then it means that $a$ and $N$ are coprimes and the quantum algorithm should be performed to find the period $r$ of a function $f(x)=a^x mod N$ for any $x\in Z$, such that $f(x)=f(x+r)$
5. **Quantum part of Shor's Algorithm**
6. We turn $y/N$ into a irreducible fraction until obtain the denominator $r'$ which is a candidate for $r$. If $f(x)=f(x+r')$ then $r'=r$ and we are done.
7. Now that we know the perior $r$ we can find the factors $q$ and $p$ from:
$$(a^{r/2}-1)(a^{r/2}+1)=pq=N$$

### Quantum part (Finding the period $r$)
5. Quantum algorithm
  1. Create an initial quantum state with two registers in the base state, each with $n\approx log_2(N)$ qubits (since we require $n$ qubits to represent a number $N$): $$|\Psi_0\rangle=|\psi_1\rangle\otimes|\psi_2\rangle =|0\rangle^{\otimes n}\otimes |0\rangle^{\otimes n}$$
  2. Apply $n$-Hadamard gates to the first register to obtain the states superposition: $$|\Psi_1\rangle=(H^{\otimes n}\otimes I^{\otimes n})|\Psi_0\rangle=H^{\otimes n}|0\rangle^{\otimes n}\otimes I^{\otimes n}|0\rangle^{\otimes n}=\frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}\left(|x\rangle\right)\otimes|0\rangle^{\otimes n}$$
  3. We apply the modular function $f(x)=a^x mod N$ to the quantum state $|x\rangle$ and save the result in the second register. To do this, we perform a control-U gate where U contains this functions, the contraol is the first register and the target is the second:
  $$|\Psi_2\rangle=CU_{f(x)}|\Psi_1\rangle=\frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}\left(|x\rangle\otimes|f(x)\rangle\right)$$
  4. Then, apply the Quantum Fourier Transform (QFT) to the first register trhough an unitary transformation where $U_{QFT}|x\rangle=\frac{1}{\sqrt{2^n}}\sum_{y=0}^{2^n-1}e^{\frac{2\pi i xy}{2^n}}|y\rangle$, so that:
  $$|\Psi_4\rangle=U_{QFT}\otimes I^{\otimes n}|\Psi_3\rangle=\frac{1}{2^n}\sum_{x=0}^{2^n-1}\sum_{y=0}^{2^n-1}\left(e^{\frac{2\pi i xy}{2^n}}|y\rangle\otimes|f(x)\rangle\right)$$
  5. Finally a measurement is performed in both registers to obtain some outcome $y$ in the first register and a $f(x_0)$ in the second. Since $f$ is periodic, the probability to measure some $y$ would be:
  $$P(y)=\frac{1}{2^n}\left|\sum_{x:f(x)=f(x_0)}e^{\frac{2\pi i xy}{2^n}}\right|^2=\frac{1}{2^n}\left|\sum_{b}e^{\frac{2\pi i (x_0+rb)y}{2^n}}\right|^2$$
  , where $b$ is an integer.

