Four broad categories:
Classification
Regression
Reinforcement Learning
Generative Learning

# Overview and Definitions
Machine Learning is the subfield of computer science that gives "computers the ability to learn without being explicitly programmed."

*Without being explicitly programmed* 
The line between machine learning and curve fitting or modelling can be a blurry one.
Reason for ML:
1. Greater Need - business need
2. More computing power
3. More data
4. Better algorithms

"A computer program is said to learn from its experience E with respect to some class of tasks T and performance measure P if its performance at tasks in T, as measured by P, improves with experience E."

Training - the process by which a machine learning system "learns" from the data which it is provided.
Optimizing - Training usually involves the ML system optimizing a set of parameters based on the data which it is provided.

## Categorization of machine learning systems

Whether or not they are trained with human supervision:
1. Supervised
2. Unsupervised
3. Semisupervised
4. Reinforcement
Whether or not they can learn incrementally
5. Online Learning
6. Batch learning
Whether they work by comparing new data points to previously known data points or by detecting patterns and building a predictive model:
7. Instance-based learning
8. Model-based learning

**Examples of supervised learning algorithms**:
1. k-nearest neighbors
2. Linear regression
3. Logistic Regression
4. Support Vector Machines
5. Decision Trees
6. Random forests
7. Neural Networks

**Unsupervised**
1. Cluster:
	1. k-Means
	2. Hierarchical Cluster Analysis
	3. Expectation Maximization
2. Visualization and Dimensionality Reduction
	1. Principal Component Analysis
	2. Kernel PCA
	3. Locally-linear embeding
	4. t-distributed Stochastic Neighbor Embedding (t-SNE)
3. Association Rule LEarning
	1. Apriori
	2. Equivelance Class Learning (ECLAT)

### **Types of parameters**
Model parameters
       - Parameters learned through model training
    - E.g.
        - Slope and intercept of the result of a least-squares fit to the training data 
Hyperparameters
    Parameters that define the model or how model training occurs
    - E.g.
        - Training or learning rate
        - Number of leaves of depth of a tree
        - Number of hidden layers in a deep neural network
        - Number of clusters in a k-mean clustering

# Broad Categories of Machine Learning

Classification: Predicting to which class (category) a sample belongs based on its features.
Regression: predicting the value of an attribute of sample based on its features.
Reinforcement: Learning to perform a task better based on experience, where "better" can be quantified
Generative: Learning to generate a sample that is drawn from the same distribution as a given set of samples are drawn from. 

# Assumptions Made in Learning a Function and Some Challenges
## Learning a Function

Given, ${\left\{ {{x^i},{y^i}} \right\}_k}$,

Find $f\left( {x,{{\hat \theta }_k}} \right)$, such that $\hat y = f\left( {x,{{\hat \theta }_k}} \right) \approx y$,
where ${{\hat \theta }_k}$ is a vector of parameters.
We want $\hat y \approx y$ for all $\left( {x,y} \right)$ drawn from the same distribution from which ${\left\{ {{x^i},{y^i}} \right\}_k}$ was drawn.
Typically, we find ${{\hat \theta }_k}$ by minimizing a loss function ${L_k}\left( {{{\left\{ {{{\hat y}^i},{y^i}} \right\}}_k},{\theta _k}} \right);$

${{\hat \theta }_k} = \mathop {{\rm{argmin}}}\limits_{{\theta _k}} {\rm{ }}{L_k}\left( {{{\left\{ {{{\hat y}^i},{y^i}} \right\}}_k},{\theta _k}} \right).$

- We have **data points**:  
  - Written as `{(x^i, y^i)}_k`  
  - Each example has an **input** `x^i` and a **true output** `y^i`.

- We want to build a **function** `f(x, θ)`:
  - It takes input `x` and some parameters `θ`.  
  - It gives a **prediction**: `ŷ = f(x, θ)`.

- Goal:  
  - Make predictions `ŷ` as close as possible to the true outputs `y`.  
  - i.e. `ŷ ≈ y`.

- Parameters (`θ`):  
  - These are the numbers we adjust (like weights in linear regression or a neural network).  
  - The "best" parameters are written as `θ̂`.

- Loss function (`L`):  
  - A formula that measures how wrong the predictions are.  
  - Example: squared error `(ŷ - y)^2`.  
  - Lower loss = better predictions.

- Training = finding the best parameters:  
  - We find `θ̂` by minimizing the loss:  
    ```
    θ̂ = argmin_θ L(predictions, true values)
    ```
  - "argmin" means: choose the `θ` that makes the loss as small as possible.

## Machine Learning makes a lot of assumptions


$y{\rm{ }} \approx \hat y = f\left( {x,{{\hat \theta }_k}} \right)$

${{\hat \theta }_k} = \mathop {{\rm{argmin}}}\limits_{{\theta _k}} {\rm{ }}{L_k}\left( {{{\left\{ {{{\hat y}^i},{y^i}} \right\}}_k},{\theta _k}} \right).$

### Assumptions
1. - ${\left\{ {{x^i},{y^i}} \right\}_k}$ is representative of distribution from which it is drawn
2. - ${\left\{ {{x^i},{y^i}} \right\}_k}$ is a large enough sample
3. - $f$ can represent the relationship between $x$ and $y$
4. - Minimizing ${L_k}\left( {{{\left\{ {{{\hat y}^i},{y^i}} \right\}}_k},{\theta _k}} \right)$ will lead to a "good" solution
5. - We can find a ${{\hat \theta }_k}$ that is a "good" solution
6. - Given ${x^i}{\rm{, }}f\left( {x,{{\hat \theta }_k}} \right)$ does a job of predicting ${{\hat y}^i}$
7. - $f\left( x \right)$ is stationary (i.e., invariant over time)
    

### Overfitting
- Developing a model that corresponds too closely (or even exactly) to the training data. If a model is overfit, it is likely to do a poor job of making predictions about other data samples drawn from the same distribution.
- In the Extreme, an overfit model simply memorizes the training data, rather learning the underlying structure of the data.
- Overfitting can often be detected by testing a model using data that is independent from the training data while being drawn from the same distribution as the training data.
- A variety of techniques (including regularization and pruning) can be used to reduce overfitting.
    

### Typical Supervised Learning Process Flow
1. start with a set of labeled data
2. Segment data into three sets: training data, testing data, and validation data
3. After looking at the data, we chose one or more machine learning algorithms; we’re using the index k to distinguish among the various models that we’ve chosen.
4. We then train each of the algorithms by minimizing a loss function that is a function of the actual values of the dependent variable or variables, our predicted values for the dependent variables,
5. We then test each of our models using the test data set.  If a model does a better job of prediction on the training data than on the test data, the model may have overfit the data.
6. We then compare how well each of the models did using the third data set, the validation data. We pick the model that did the best job as the one we’ll use to predict the values of the dependent variables for new data samples.
7. If we’re given new data, we use the model we’ve selected as the best one to predict the value of the dependent variable or variables.

### Principle Challenges in ML
1. Insufficient training data
2. Non-representative training data
3. Incomplete or poor data quality
4. Irrelevant features
5. Over-fitting the training data
6. Underfitting the training data
7. Non-stationary data
8. Some models are "black boxes"

### No Free Lunch Theorem
If yo make absolutely no assumptions about the data, then there is no reason to prefer one model over another.
That is, one size does not fit all.
Proved by David Wolpert in 1996.

### Ockham's Razor
Given multiple explanations for an occurrence, the simpler one is usually better
"Pluralitas non est ponenda sine necessitate," i.e., "Plurality is not to be posited without necessity."
