# Gaussian process

We can use a Gaussian process to describe a distribution over functions : 
$$f \sim \mathcal{GP}(m,K)$$
where $m$ is the mean function $m(x) = \mathbb{E}[f(x)]$ and K is the covariance function $K(x,x')=\mathbb{E}[(f(x)-m(x))(f(x')-m(x'))]$

> Note the similarity with the gaussian distribution $X \sim \mathcal{N}(\mu, \sigma)$

## Multivariate Gaussian distributions
We have data points $X=[x_1^T, ...,x_n^T]^T$ and are interested in their functions values $f(X)=(f(x_1), ..., f(x_n))^T$
A GP is a collection of random variables (RV), any finite number of which have joint Gaussian distribution.
$f$ is one such subset of RV and has joint Gaussin distribution
$f(X) \sim \mathcal{N}(m(X), K(X,X))$
 
### Idea
Assumption is that if vectors $x$ and $x'$ are similar, then $f(x)$ and $f(x')$ should be similar too
The covariance function $K(x,x')$ returns a mesure of the similarity of $x$ and $x'$ that also encodes how similar $f(x)$ and $f(x')$ should be.
The mean function $m(x)$ encodes the a priori expectation of the unknown function.
For inference, we condition the unknown function values on the known ones, If there are no "similar" known values, then the mean function dominates the result.

### Trick
If we normalize the output to zero mean, we can simply use $\mathbb{E}[f(x)]=m(x)=0$

## Properties of the covariance function
Recall, the covariance function $K(x,x')$ need to be a measure of similarity between $x$ and $x'$. This is the basic assumption which makes the inference possible sine we assume that similar data points have similar function values.

$K$ needs to be *symmetric* and positive semidefinite (see Mercer's theorem and kernel lessons)

### Setting the covariance function
The "default" covariance function is the squared exponential kernel $K(x,x')=\sigma^2 \exp(-\frac{(x-x')^T(x-x')}{2l^2})$ where $l$ varies the lenght or width and $\sigma$ the height of the kernel/
> Build a courbe k(0,x) with x between 0 and 1 and $\sigma \in {0.5,1}$

The covariance function is what drives the behavior of a GP. Remember, it is not X, over which the Gaussian is defined but $f(X)$

### Interpretation
If two points are similar, they convary strongly. Knowing about $f_1$ reveals a lot about $f_2$
I two points are far apart, their covariance is small. Knowing the values of f1 reveals little about $f_2$.

## Use GP for regression
### Drawing samples from a multivariate normal
Sampling from the prior distribution of a GP at arbitrary points $X_*$
$$ f_{\text{pri}}(X_*) \sim \mathcal{GP}(m(X_*), K(X_*,X_*))$$ is equivalent to sampling from a multivariate normal $$ f_{\text{pri}}(X_*) \sim \mathcal{N}(m(X_*), K(X_*,X_*))$$

### Inference with GPs - noise-free case
We have training data X ($N \time D$), corresponding observation $f=f(X)$ and test data points $X_*$ for which we want to infer function values $f_*=f(X_*)$
The GP defines a joint distribution for $p(f, f_* | X, X_*)$:
![]()

To infer $f_*$ or rather $p(f_*|X_*,X, f)$, we apply the rules for conditioning multivariate Gaussians and obtains 
![]()

In the noise-free case the GP acts as an interpolator between observed values. More often, the assumption that our observation $y_i$ corrspond exactly to the function values $f(x_i) = f_i$ is wrong. We will now instead assume that we observe a noisy version of the underlying function $y_i = f_i+\epsilon$ where $\sigma \sim \mathcal{N}(0,\sigma^2_y) is additive iid Gaussian noise.


 