# Fractals
## Mandelbrot set

The Mandelbrot set $\mathcal{M}$ is defined as
$$
\textrm{with} \quad c \in \mathbb{C} \quad \textrm{and} \quad \left\{
\begin{array}{l}
z_0 = 0 \\
z_{n+1} = z_n^2 + c
\end{array}
\right.
$$
$$ c \in \mathcal{M} \  \Leftrightarrow \  \exists L \in \mathbb{R} \quad \forall n \in \mathbb{N} \quad |z_n| \le L $$
## Julia set
The Julia set $\mathcal{J}_c$ is defined as
$$
\textrm{with} \quad c \in \mathbb{C} \quad \textrm{and} \quad \left\{
\begin{array}{l}
z_0 \in \mathbb{C} \\
z_{n+1} = z_n^2 + c
\end{array}
\right.
$$
$$ z_0 \in \mathcal{J}_c \  \Leftrightarrow \  \exists L \in \mathbb{R} \quad \forall n \in \mathbb{N} \quad |z_n| \le L $$
# Riemann

The Riemann series converges $\forall s \in \mathbb{C}$ where $Re(s) > 1$ :

$$ \zeta(s) = \sum_{n=1}^{\infty}\frac{1}{n^s} $$
The $\zeta$ function is defined for values $\operatorname{Re}(s) \leq 1$ by analytic continuation. 
This gives odd behavior, like :
$$ \zeta(0) = -\frac{1}{2} \sim \sum_{n=1}^{\infty}1 = 1 + 1 + 1 + \dots $$
$$ \zeta(-1) = -\frac{1}{12} \sim \sum_{n=1}^{\infty} n = 1 + 2 + 3 + \dots $$
# Gamma function
$$
\forall z \in \mathbb{C} \  | \  \operatorname{Re}(z) > 0 \quad \Gamma(z) = \int_0^{+\infty}t^{z-1}e^{-t}dt
$$
It’s a generalization of the factorial operator :
$$
\forall n \in \mathbb{N} \quad \Gamma(n)=(n-1)!
$$
# Équation de Kaya

$$ CO_2 = Pop \times \frac{PIB}{Pop} \times \frac{E}{PIB} \times \frac{CO_2}{E} $$
Avec :
- $CO_2$ : émissions anthropiques mondiales de CO2
- $Pop$ : population mondiale
- $PIB$ : produit intérieur brut mondial
- $E$ : consommation mondiale d’énergie primaire
Il en découle :
- $\frac{PIB}{Pop}$ : niveau de vie moyen
- $\frac{E}{PIB}$ : intensité énergétique du PIB (quantité d’énergie nécessaire pour produire 1€ de bien ou service)
- $\frac{CO_2}{E}$ : intensité carbone de l’énergie