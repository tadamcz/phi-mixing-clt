# Escaping variance in a stationary $`\phi`$-mixing sequence

## 1. The result and the strategy

The construction separates the typical size of a partial sum from its $`L^2`$ size. Bounded feedback suppresses the contribution of each fixed finite part of the innovation law. Successively rarer and larger innovations carry the remaining variance.

**Theorem.** There exists a strictly stationary, centered, $`\phi`$-mixing sequence $`(X_n)_{n\ge0}`$ with $`\mathbb E X_0^2\lt \infty`$ such that, writing

```math
S_n=\sum_{t=0}^{n-1}X_t,\qquad \sigma_n^2=\mathrm{Var}(S_n),
```

one has $`\sigma_n^2\to\infty`$, but along a strictly increasing sequence $`(n_j)`$,

```math
\frac{S_{n_j}}{\sigma_{n_j}}\longrightarrow0
\qquad\text{in probability}.
```

In particular, the variance-normalized central limit theorem fails.

The argument has four main ingredients:

- **Feedback and attenuation (§§2–3).** A finite cascade can suppress ordinary fluctuations at arbitrarily small amplitude and variation cost.
- **Uniform forgetting (§4).** Square-summable variation of the inverse rule yields mixing and permits comparison of stationary block laws.
- **Rare spikes (§5).** Innovations with prescribed second-moment contributions give a variance lower bound that bounded feedback cannot erase.
- **The diagonal choice (§§6–7).** Each observation time is fixed before the next spike is introduced, so typical sums become small while their variance survives.

## 2. A bounded feedback controller

We construct the sequence on a two-sided iid innovation space. Its forward and inverse descriptions will be

```math
X_t=\xi_t+K(\xi_{t-1},\xi_{t-2},\ldots),
\qquad
\xi_t=X_t+H(X_{t-1},X_{t-2},\ldots).
\qquad\text{(1)}
```

Here $`K`$ and $`H`$ are bounded, continuous, odd functions on the space of real histories, equipped with the product topology. The forward description supplies stationarity and integrability. The inverse description exposes the conditional laws needed to prove mixing. Both identities will hold at every innovation history.

Begin with a symmetric square-integrable innovation law $`\nu`$. Suppose an existing causal process is

```math
Y_t=\xi_t+K_{\rm old}(\xi_{t-1},\xi_{t-2},\ldots).
```

For parameters $`N\ge1`$, $`a\ge0`$, and $`s\gt 0`$, with $`g=aN\lt 1`$, introduce the feedback rule

```math
X_t=Y_t-a\,C_s\!\left(\sum_{\ell=1}^{N}X_{t-\ell}\right),
\qquad
C_s(u)=\max(-s,\min(s,u)).
\qquad\text{(2)}
```

Regarded as an equation for the new bounded correction $`K`$, this is a contraction in the supremum norm with constant $`g`$. Thus it has a unique bounded continuous solution, and

```math
\|K-K_{\rm old}\|_\infty\le as.
\qquad\text{(3)}
```

Oddness is preserved. The construction is deterministic and therefore works simultaneously for every innovation law.

### The inverse map and the two budgets

The inverse of this single feedback step is explicit:

```math
(\mathcal T x)_t
=x_t+a\,C_s\!\left(\sum_{\ell=1}^{N}x_{t-\ell}\right).
```

It changes each coordinate by at most $`b=as`$ and has Lipschitz bound $`1+g`$ for uniform differences of histories. For a cascade starting from $`X_t=\xi_t`$, with controllers indexed by $`i=1,\ldots,\ell`$, put

```math
M_i=\sum_{r\le i}N_r,\qquad
G_i=\prod_{r\le i}(1+g_r),\qquad
\beta_i=G_{i-1}a_i s_i,\qquad M_0=0,\quad G_0=1.
```

Its inverse correction $`H_i`$ depends on the first $`M_i`$ entries of the observed past, and

```math
\|H_i-H_{i-1}\|_\infty\le\beta_i,
\qquad
\|K_i-K_{i-1}\|_\infty\le\beta_i.
\qquad\text{(4)}
```

The factor $`G_{i-1}`$ accounts for the effect of the new controller on all the preceding inverse maps.

Two quantities will be budgeted throughout:

```math
\mathcal A=\sum_i\beta_i,\qquad
\mathcal C=\sum_i M_i\beta_i^2.
\qquad\text{(5)}
```

The first ensures uniform convergence of the forward and inverse corrections. The second will control mixing. We always choose $`N_i\ge M_{i-1}`$, so that the cumulative memories at least double.

## 3. Attenuation at arbitrarily small cost

**Lemma 1 (Cheap attenuation).** Fix a symmetric square-integrable innovation law and an existing finite cascade with doubling cumulative memories. For every $`\tau,\eta,\beta\gt 0`$, the cascade has a nonempty finite extension whose output satisfies

```math
\|S_n\|_2\le\tau\sqrt n+C\qquad(n\ge1)
\qquad\text{(6)}
```

for some finite $`C`$, while increasing $`\mathcal C`$ by at most $`\eta`$ and $`\mathcal A`$ by at most $`\beta`$. The extension can preserve the doubling of cumulative memories.

The proof has three parts: control the propagation of an innovation, estimate the error caused by saturation, and distribute the attenuation over many weak controllers.

### 3.1. Influence bounds

A continuous odd causal observable $`F`$ has influence envelope $`d=(d_k)_{k\ge0}`$ if changing its input at lag $`k`$ by $`u`$ changes its value by at most $`d_k|u|`$. Suppose $`d_k\ge0`$ and $`D=\sum_k d_k\lt \infty`$. Write $`S_n(F)=\sum_{t\lt n}F(\xi_t,\xi_{t-1},\ldots)`$. With $`v=\|\xi_0\|_2`$, the Efron–Stein inequality gives

```math
\|S_n(F)\|_2\le vD\sqrt n.
\qquad\text{(7)}
```

Indeed, the influence of one innovation on a block sum is the convolution of $`d`$ with the indicator of that block; the squared $`\ell^2`$ norm of this convolution is at most $`nD^2`$. Apply Efron–Stein first to histories with only finitely many nonzero coordinates and then pass to the limit by Fatou. Symmetry and oddness center these approximations.

Under (2), an envelope $`d`$ for $`Y`$ becomes the renewal envelope

```math
e_k=d_k+a\sum_{\ell=1}^{N}e_{k-\ell},
\qquad e_k=0\quad(k\lt 0),
```

whose mass is

```math
E=\sum_{k\ge0}e_k=\frac{D}{1-g}.
\qquad\text{(8)}
```

Starting from the current innovation, with envelope $`d_0=1`$, $`d_k=0`$ for $`k\gt 0`$, this proves that every finite cascade has a finite square-root upper bound.

### 3.2. The saturation residual

To see the attenuation, set

```math
W_t=\sum_{\ell=1}^{N}X_{t-\ell},
\qquad
R_t=a\bigl(W_t-C_s(W_t)\bigr).
```

Summing the controller equation gives

```math
(1+g)S_n(X)=S_n(Y)+S_n(R)+B_n,
\qquad
\sup_n\|B_n\|_2\le aN(N+1)\|X_0\|_2.
\qquad\text{(9)}
```

The boundary term consists of the two endpoint blocks left by each lag. Its size may depend on the controller, but is independent of $`n`$.

The residual $`R`$ requires a more careful estimate. Let $`\xi_0'`$ be an independent copy of $`\xi_0`$, and define

```math
q(L)=
\left\|(\xi_0'-\xi_0)
  \mathbf1_{\{|\xi_0'-\xi_0|\gt L\}}\right\|_2.
```

Finite second moment gives $`q(L)\to0`$. Write $`r_s(u)=u-C_s(u)`$. If replacing one innovation changes a window from $`u`$ to $`u'`$, with
$`|u-u'|\le w|\xi_0'-\xi_0|`$, then

```math
|r_s(u)-r_s(u')|
\le w\left(
|\xi_0'-\xi_0|\mathbf1_{\{|\xi_0'-\xi_0|\gt L\}}
+\frac{L}{s}(|u|+|u'|)
\right).
\qquad\text{(10)}
```

This follows from the nonexpansiveness of $`r_s`$ and its vanishing on $`[-s,s]`$.

The window has influence mass $`NE`$. Moreover, (7) bounds its $`L^2`$ norm, and that of each finite approximation and its resampled copy, by $`vE\sqrt N`$. Applying Efron–Stein to (10) therefore yields

```math
\|S_n(R)\|_2
\le
gE\left(q(L)+\frac{2LvE\sqrt N}{s}\right)\sqrt n.
\qquad\text{(11)}
```

This estimate uses only second moments. In particular, the small-replacement term involves the $`L^2`$ norm of the window, without introducing a fourth moment.

Choose the saturation scale as $`s=A\sqrt N`$. Combining (9) and (11), a square-root slope $`u`$ for the old process becomes

```math
\frac{u+gE\bigl(q(L)+2LvE/A\bigr)}{1+g}.
\qquad\text{(12)}
```

The window length has disappeared from this expression.

### 3.3. Many weak controllers give a cheap batch

Now take a batch of $`m\ge2`$ controllers, each with gain $`g=1/(2m)`$. Throughout the batch, the influence mass and inverse Lipschitz bound are at most twice their initial values. This follows from

```math
\prod(1-g)^{-1}\le\frac1{1-mg}=2,
\qquad
\prod(1+g)\le\frac1{1-mg}=2.
```

Given an initial slope $`T\gt 0`$, choose $`L`$, and then $`A`$, so that

```math
E\bigl(q(L)+2LvE/A\bigr)\le T/8
```

throughout the batch. Formula (12) then admits the successive envelope slopes $`T(1-kg/2)`$, $`0\le k\le m`$: for $`u\ge3T/4`$ and $`g\le1/4`$, one has $`(u+gT/8)/(1+g)\le u-Tg/2`$. After $`m`$ steps the slope is at most $`3T/4`$.

Meanwhile, if $`G`$ is the inverse Lipschitz bound before a particular step and $`M\le N`$ is the preceding memory, that step has amplitude

```math
\beta_{\rm step}=\frac{GgA}{\sqrt N}
```

and cost

```math
(M+N)\beta_{\rm step}^2\le2(GgA)^2.
```

Consequently, the total batch cost is at most $`2G_{\rm initial}^2A^2/m`$. Choose $`m`$ large enough to meet the cost budget, and then choose each window large enough to meet the amplitude budget and the memory constraint. Repeating finitely many such batches proves Lemma 1. $`\square`$

## 4. From square variation to uniform forgetting

The role of the variation cost is to make the distant past uniformly negligible, even when the entire future is observed.

### 4.1. The variation cost

We next explain why the cost in (5) is the right one. For a bounded history function $`H`$, write

```math
\mathrm{var}_m(H)=
\sup\{|H(x)-H(y)|:x\text{ and }y
\text{ agree in their first }m\text{ entries}\}.
```

**Lemma 2 (Control of the inverse variation).** An infinite cascade with doubling cumulative memories and $`\sum_i\beta_i\lt \infty`$ has an inverse limit satisfying

```math
\mathrm{var}_m(H)\le d_m,
\qquad
d_m=2\sum_{i:M_i\gt m}\beta_i.
\qquad\text{(13)}
```

Its variation majorant satisfies

```math
\sum_{m\ge0}d_m^2
=4\sum_{i,k}\beta_i\beta_k\min(M_i,M_k)
\le24\sum_iM_i\beta_i^2.
\qquad\text{(14)}
```

**Proof.** Each inverse increment has norm at most $`\beta_i`$ and memory $`M_i`$, which gives (13). To control the cross terms in (14), set $`c_i=\sqrt{M_i}\beta_i`$. The matrix multiplying $`c_ic_k`$ is bounded by $`2^{-|i-k|/2}`$, whose row sums are at most $`3+2\sqrt2\lt 6`$. This proves (14), first for finite cascades and then by monotone convergence. $`\square`$

### 4.2. Gaussian smoothing gives uniform forgetting

**Lemma 3 (Uniform forgetting).** Suppose the innovation law is the law of $`J+Z`$, where $`Z\sim N(0,1)`$ is independent of $`J`$. If the inverse rule in (1) has bounded $`H`$ and

```math
\mathrm{var}_m(H)\le d_m,\qquad
\sum_m d_m^2\lt \infty,
```

then the stationary output is $`\phi`$-mixing.

**Proof.** The Gaussian component has two complementary uses.

**Agreement controls the continuation.** Retain the latent variable $`J`$ alongside each output. For two possible drifts differing by $`u`$, the one-step Hellinger affinity of these augmented laws is exactly

```math
\exp(-u^2/8).
```

If two initial histories agree in their first $`m`$ entries, then along a common output path their drifts at step $`r`$ differ by at most $`d_{m+r}`$. Iterated conditional integration of the Gaussian square-root likelihood gives affinity at least

```math
\exp\!\left(-\frac18\sum_{r=0}^{L-1}d_{m+r}^2\right).
```

Thus the laws of the next $`L`$ outputs, denoted by $`P_x^L`$ and $`P_y^L`$, satisfy

```math
\|P_x^L-P_y^L\|_{\rm TV}
\le\left(\sum_{r=0}^{L-1}d_{m+r}^2\right)^{1/2}
\le\delta_m,
\qquad
\delta_m=\left(\sum_{r\ge m}d_r^2\right)^{1/2}.
\qquad\text{(15)}
```

We use $`\|P-Q\|_{\rm TV}=\sup_A|P(A)-Q(A)|`$.

**A common block creates agreement.** Bounded Gaussian shifts have a common minorizer. If $`\|H\|_\infty\le B`$, then

```math
N(-H(x),1)\ge q_B\,N(0,1/2),
\qquad q_B=2^{-1/2}e^{-B^2}\gt 0.
```

After convolution with the law of $`J`$, this supplies the same minorization for every conditional output law. Hence the law of a complete block of $`m`$ outputs has a common component of mass $`q_B^m`$, independent of the initial history. On this component the two updated histories have the same first $`m`$ entries, so (15) controls every subsequent continuation.

Splitting each block law into this common component and its remainder gives, after $`k`$ blocks,

```math
\sup_{x,y}
\|P_x^{\,km,L}-P_y^{\,km,L}\|_{\rm TV}
\le\delta_m+(1-q_B^m)^k.
\qquad\text{(16)}
```

Here $`P_x^{\,b,L}`$ is the law of the $`L`$ outputs following $`b`$ initial steps. The bound is independent of $`L`$. Choose $`m`$ to make $`\delta_m`$ small and then $`k`$ to make the geometric term small.

**Uniformity gives the mixing coefficient.** Conditional on any positive-probability past event, the future law is a mixture of these continuation laws; the unconditional future law is another such mixture. Their distance is bounded by the same uniform diameter. Passing from finite blocks to the sigma-algebra of the whole future proves the lemma. $`\square`$

### 4.3. Stability of stationary block laws

The diagonal construction will compare processes with different kernels and different innovation laws. The following consequence of Lemma 3 makes that comparison possible.

**Corollary 4 (Comparison after forgetting).** Consider two stationary processes with forward and inverse descriptions as in (1), driven by innovation laws $`\lambda*N(0,1)`$ and $`\lambda'*N(0,1)`$. Suppose $`H'`$ is bounded and has finite memory, and

```math
\|H-H'\|_\infty\le\delta,\qquad
\|\lambda-\lambda'\|_{\rm TV}\le e.
```

For every $`\varepsilon\gt 0`$, there is a forgetting time $`T`$, depending only on the reference process and $`\varepsilon`$, such that for every $`n`$,

```math
\|\mathcal L(S_n)-\mathcal L(S_n')\|_{\rm TV}
\le\varepsilon+(T+n)(\delta+e).
\qquad\text{(17)}
```

**Proof.** The conditional one-step laws differ by at most $`\delta+e`$: convolution contracts total variation, and unit-variance Gaussian laws are Lipschitz in their means. Telescoping over a path of length $`T+n`$ gives an error at most $`(T+n)(\delta+e)`$.

For the reference rule $`H'`$, the agreement error in (15) is zero once the whole memory agrees. Thus (16) supplies a time $`T`$ after which all continuation laws are within $`\varepsilon`$, uniformly in their length. Stationarity gives (17). The forgetting interval allows the two stationary processes to have different laws of their initial pasts. $`\square`$

## 5. Rare spikes preserve the variance

The attenuation lemma applies to any fixed finite core of the innovation law. We now design the tail so that its second-moment contribution survives the bounded feedback.

### 5.1. Prescribed energy, arbitrarily small probability

Set

```math
\varepsilon_j=2^{-(j+1)},\qquad
p_j=\frac{\varepsilon_j}{Q_j^2},
```

where the sizes $`Q_j\ge1`$ will be chosen inductively. Let

```math
\lambda=\mathcal L(J)=
\left(1-\sum_jp_j\right)\delta_0
+\sum_j\frac{p_j}{2}(\delta_{Q_j}+\delta_{-Q_j}),
\qquad \xi=J+Z,
\qquad\text{(18)}
```

with $`Z\sim N(0,1)`$ independent. The spikes are mutually exclusive components of a mixture. Their second-moment contributions are prescribed:

```math
p_jQ_j^2=\varepsilon_j,\qquad
\mathbb E\xi^2=1+\sum_j\varepsilon_j=2.
```

Increasing $`Q_j`$ makes the $`j`$-th spike arbitrarily unlikely while leaving its contribution to the second moment unchanged.

Write

```math
e_\xi(R)=\mathbb E\bigl[\xi^2\mathbf1_{\{|\xi|\gt R\}}\bigr].
```

Whenever $`R\lt Q_j`$,

```math
e_\xi(R)\ge\varepsilon_j/2.
\qquad\text{(19)}
```

Indeed, for every real $`z`$, at least one of $`z+Q_j`$ and $`z-Q_j`$ has absolute value at least $`Q_j`$. Averaging the two signs and integrating over $`z=Z`$ proves (19).

### 5.2. A tail witness gives a variance lower bound

**Lemma 5 (Variance survives bounded correction).** Let $`\xi_t`$ be iid, symmetric, and square-integrable. Suppose $`B\ge0`$ and

```math
S_n=\sum_{t\lt n}\xi_t+D_n,\qquad |D_n|\le nB.
```

Then, for every $`R\gt 0`$ with $`R\ge2nB`$,

```math
\mathrm{Var}(S_n)\ge\frac n4e_\xi(R).
\qquad\text{(20)}
```

**Proof.** Use the centered witness

```math
T_n=\sum_{t\lt n}\xi_t\mathbf1_{\{|\xi_t|\gt R\}}.
```

Independence gives

```math
\mathrm{Var}(T_n)
=\mathrm{Cov}\!\left(\sum_{t\lt n}\xi_t,T_n\right)
=ne_\xi(R).
```

On the other hand,

```math
|\mathbb E(D_nT_n)|
\le nB\,\mathbb E|T_n|
\le\frac{n^2B}{R}e_\xi(R)
\le\frac12ne_\xi(R).
```

Thus $`\mathrm{Cov}(S_n,T_n)\ge\mathrm{Var}(T_n)/2`$. Expanding the nonnegative quantity $`\mathrm{Var}(S_n-T_n/2)`$ gives (20). $`\square`$

The use of the first moment of $`T_n`$ in the error estimate is what makes rare, large innovations effective.

## 6. The diagonal construction

All ingredients are now available. The order of choices ensures that a later spike cannot invalidate an earlier comparison.

Start with no controllers and $`r_0=1/16`$. At stage $`j`$, only $`Q_0,\ldots,Q_{j-1}`$ have been chosen. Let $`\lambda_j`$ retain precisely these spikes, returning all other mass to zero, and let $`\nu_j=\lambda_j*N(0,1)`$.

**Step 1: attenuate the existing core.** Apply Lemma 1 to $`\nu_j`$, extending the preceding cascade to forward and inverse corrections $`K^{(j)},H^{(j)}`$, with target slope $`\varepsilon_j^2`$, added cost at most $`\varepsilon_j`$, and added amplitude at most $`r_j`$.

**Step 2: choose the observation time.** Choose

```math
n_j\gt n_{j-1},\qquad n_j\ge j+1,
```

large enough that the stationary sum $`S_{n_j}^{(j)}`$ formed from this cascade and the innovation law $`\nu_j`$ satisfies

```math
\mathrm{Var}(S_{n_j}^{(j)})
\le2n_j\varepsilon_j^4.
\qquad\text{(21)}
```

Such a choice is possible by absorbing the fixed boundary constant in (6).

**Step 3: choose the forgetting time.** Since $`H^{(j)}`$ has finite memory, choose a forgetting time $`T_j`$ with error $`\varepsilon_j/2`$, uniformly over all initial histories and all future block lengths.

**Step 4: fix the future budget and then introduce the new spike.** Choose

```math
0\lt r_{j+1}\le
\min\left\{\frac{r_j}{4},
\frac{\varepsilon_j}{16(T_j+n_j+1)}\right\}.
```

Only now choose $`Q_j`$, sufficiently large that

```math
Q_j\ge4^{j+1},\qquad
Q_j\gt 16n_j,\qquad
p_j=\varepsilon_j/Q_j^2\le r_{j+1}.
\qquad\text{(22)}
```

This order of choices is essential: the attenuation and forgetting estimates concern a law determined entirely by earlier stages.

### The limiting process

The total cost is at most $`\sum_j\varepsilon_j=1`$, and the total amplitude is at most $`\sum_jr_j\le1/8`$. Equations (4) therefore give uniform limits $`K,H`$, both bounded by $`1/8`$, continuous, and odd. The finite reconstruction identities pass to the limit, giving (1). Equations (13)–(14) give square-summable variations for $`H`$, so the forgetting lemma proves $`\phi`$-mixing. The forward representation immediately gives strict stationarity, centering, and finite second moment.

### Preserving the selected observations

The later stages have been made small enough to preserve each selected observation. Indeed,

```math
\|H-H^{(j)}\|_\infty\le2r_{j+1},
\qquad
\|\lambda-\lambda_j\|_{\rm TV}
\le\sum_{i\ge j}p_i\le2r_{j+1}.
```

Using (17) with the forgetting time $`T_j`$ yields

```math
\|\mathcal L(S_{n_j})-\mathcal L(S_{n_j}^{(j)})\|_{\rm TV}
\le\varepsilon_j.
\qquad\text{(23)}
```

At the same time, the new spike lies beyond $`16n_j`$. Equations (19)–(20), with the bound $`\|K\|_\infty\le1`$, give

```math
\sigma_{n_j}^2\ge n_j\varepsilon_j/8.
\qquad\text{(24)}
```

## 7. Completion of the proof

Two assertions remain: variance divergence at every large time, and collapse of the normalized laws along the selected subsequence.

### 7.1. Variance diverges along the whole sequence

For an arbitrary observation time $`n`$, let

```math
k(n)=\min\{j:Q_j\gt 16n\}.
```

Every finite collection of spike sizes is eventually below $`16n`$, so $`k(n)\to\infty`$. By minimality and $`Q_j\ge4^{j+1}`$, one has $`4^{k(n)}\le16n`$ for $`n\ge1`$. Applying (19)–(20) to the spike with index $`k(n)`$ gives

```math
\sigma_n^2
\ge\frac n8\,2^{-(k(n)+1)}
\ge\frac{2^{k(n)}}{256}
\longrightarrow\infty.
\qquad\text{(25)}
```

### 7.2. The normalized sums collapse along the selected subsequence

The finite-core sums are centered. For every $`a\gt 0`$, (21), (23), (24), and Chebyshev's inequality imply

```math
\begin{aligned}
\mathbb P\!\left(|S_{n_j}|\gt a\sigma_{n_j}\right)
&\le\varepsilon_j+
\frac{\mathrm{Var}(S_{n_j}^{(j)})}{a^2\sigma_{n_j}^2}\\[2pt]
&\le\varepsilon_j+\frac{16}{a^2}\varepsilon_j^3
\longrightarrow0.
\end{aligned}
```

This proves the theorem. $`\square`$

**Consequence for the invariance principle.** The polygonal partial-sum processes cannot converge to standard Brownian motion either, since evaluation at time $`1`$ would give the excluded scalar central limit theorem.

**The mechanism of failure.** Along the selected subsequence, the normalized sums have second moment one while converging in probability to zero. Their second moments escape onto events of vanishing probability.
