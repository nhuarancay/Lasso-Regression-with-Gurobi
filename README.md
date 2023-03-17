# Lasso-Regression-with-Gurobi

This work is focused on how to **set coefficients for linear regression**.

The **Step-by-Step Guide to Formulate Lasso Linear Regression with Gurobi and Setting Coefficients**, can be better explain in this article: https://medium.com/@nicolay.huarancay/lasso-regression-with-gurobi-and-setting-coefficients-approach-179be20eea14

Linear regression is a popular technique with many real-world applications. However, the question of whether it is appropriate to set coefficients in multiple regression is controversial, as it can lead to biases and suboptimal solutions. Various techniques such as lasso and robustness can still produce effective outcomes. The decision to set coefficients depends on factors such as prior knowledge, improving interpretability, and additional techniques like feature selection and regularization. 

While exploring this topic, I wrote an article on formulating Lasso linear regression and setting coefficients in a step-by-step approach, but it's important to note that this can lead to issues that require addressing. Basic knowledge of linear and Lasso regression theory, linear algebra, and optimization models is necessary to understand the article.
<br><br>

There are some Linear Regression solution formulations:

### Solution Formulation - Regular Linear Regression:

$$ \min_{\beta} \; \sum_{i=1}^{n} (\beta_0 + \beta_1x_{i1} + ... + \beta_1x_{ip}  -  y_i)^2 $$

$$ \text{Matrix notation:} $$

$$ \min_{\beta} \; \beta^\mathsf{T}(\mathbf{X}^\mathsf{T} \mathbf{X})\beta -2\beta^\mathsf{T} \mathbf{X}^\mathsf{T} \mathbf{y} + \mathbf{y}^\mathsf{T} \mathbf{y} $$

### Solution Formulation - Lasso Linear Regression:

$$ \min_{\beta} \; \sum_{i=1}^{n} (\beta_0 + \beta_1x_{i1} + ... + \beta_1x_{ip}  -  y_i)^2 + \sum_{j=1}^{m} \lambda_j \lvert{\beta_j}\rvert $$

$$ \text{Matrix notation:} $$

$$ \min_{\beta} \; \beta^\mathsf{T}(\mathbf{X}^\mathsf{T} \mathbf{X})\beta -2\beta^\mathsf{T} \mathbf{X}^\mathsf{T} \mathbf{y} + \mathbf{y}^\mathsf{T} \mathbf{y} + \lambda \lvert{\beta}\rvert $$

$$ \text{where:} $$

$$ \lambda = \text{lambda vector} $$

$$ \lambda_1 = 0 $$

$$ \lambda_{j \neq 1} = l_{1} \text{ penalization (alpha)} $$

### Formulation Lasso MIQP:

$$ \min_{\beta^{+}, \beta^{-}, z} 
\begin{pmatrix} 
\beta^{+}\\
\beta^{-}\\ 
\mathbf{z} 
\end{pmatrix}^\mathsf{T} 
\begin{pmatrix}
+\mathbf{X}^\mathsf{T}\mathbf{X} & -\mathbf{X}^\mathsf{T}\mathbf{X} & 0 \\
-\mathbf{X}^\mathsf{T}\mathbf{X} & +\mathbf{X}^\mathsf{T}\mathbf{X} & 0 \\
0 & 0 & 0 
\end{pmatrix}
\begin{pmatrix}
\beta^{+}\\
\beta^{-}\\
\mathbf{z}
\end{pmatrix} +
\begin{pmatrix}
\beta^{+}\\
\beta^{-}\\
\mathbf{z}
\end{pmatrix}^\mathsf{T}  
\begin{pmatrix}
\lambda - 2\mathbf{X}^\mathsf{T}\mathbf{y} \\ 
\lambda + 2\mathbf{X}^\mathsf{T}\mathbf{y} \\ 
0 
\end{pmatrix} +
\mathbf{y}^\mathsf{T}\mathbf{y}
$$

$$ \text{subject to:} $$

$$ \beta^+_i + M z_i \leq 0 $$

$$ \beta^{-}_i - M(1-z_i) \leq 0 $$

$$ \text{where:} $$

$$ \beta^{+} \geq 0  ;  \beta^{+}: \text{positive betas} $$

$$ \beta^{-} \geq 0  ;  \beta^{-}: \text{negative betas} $$

$$ z_i: \text{ binary, 1 if } \beta^{+}_{i} > 0 \text{, and 0 if } \beta^{-}_{i} \geq 0 $$

$$ M: \text{big number} $$
