# Perceptron Introduction
Imagine using the human brain as a model for developing artificial intelligence
The perceptron illustrates a number of issues that remain central to ML today.

# An Early Machine Learning Model
Scientists have wanted to use the human brain as a model - the neuron (receives and transmits nerve impulses)

View of the neuron as a simple logic gate with binary outputs

Perceptron is a binary classifier - inputs are the features of the object to be classified, output is whether object is {1, 0}

# Perceptron Black Box
Perceptron can be viewed as a black box - inputs go in, an output comes out

Input is assigned a weight, the inputs are multiplied by the weight, and then summed up. This sum goes into an activation function where the output is either 0 or 1

# Activation Function
$\tilde{z}=\mathrm{w} \cdot \mathrm{x}=\mathrm{w}^{T} \mathrm{x}=\sum_{i=1}^{m} w_{i} x_{i}$

is just different ways of writing the **dot product** of two vectors.

- **w** = weights = \([w_1, w_2, ..., w_m]\)  
- **x** = inputs = \([x_1, x_2, ..., x_m]\)  

The dot product means:

1. Multiply each pair of numbers: \(w_1x_1, w_2x_2, ..., w_mx_m\)  
2. Add them all together.  

So:

\[
w \cdot x = w_1x_1 + w_2x_2 + ... + w_mx_m
\]

take each input, multiply it by its weight, then add everything up.


1. **Start with the net input**  
   We defined:
   \[
   ${\tilde{z} = \sum_{i=1}^m w_i x_i}$
   \]
   This means: multiply each input \(x_i\) by its weight \(w_i\), then add them all up.

---

2. **Subtract the threshold (λ)**  
   The activation function compares \(\tilde{z}\) against a threshold λ.  
   To make the math easier, we subtract λ from both sides:
   \[
   ${\tilde{z} - \lambda}$
   \]

---

3. **Introduce \(x_0\) and \(w_0\)**  
   - Define \(x_0 = 1\).  
   - Define \(w_0 = -\lambda\).  

   Then:
   \[
   ${w_0 x_0 = -\lambda}$
   \]

   So instead of writing “subtract λ,” we can write it as just another weight–input product.

---

4. **Extend the summation index**  
   Originally the sum went from \(i = 1\) to \(m\).  
   Now we can include the \(w_0x_0\) term by starting at \(i=0\):  
   \[
   ${z = \sum_{i=0}^m w_i x_i}$
   \]

   This cleans up the formula: the threshold is “absorbed” into the weights.

---

5. **Activation function**  
   Originally:
   \[
   \phi(\tilde{z}) =
   \begin{cases}
   1 & \tilde{z} \geq \lambda \\
   0 & \tilde{z} < \lambda
   \end{cases}
   \]

   After redefining \(z = \tilde{z} - \lambda\), this becomes:
   \[
   \phi(z) =
   \begin{cases}
   1 & z \geq 0 \\
   0 & z < 0
   \end{cases}
   \]


- We start with weighted inputs compared against a threshold λ.  
- To simplify, we “fold” the threshold into the weights by inventing a dummy input \(x_0 = 1\) with weight \(w_0 = -\lambda\).  
- This lets us write everything as one neat sum:
  \[
  ${z = \sum_{i=0}^m w_i x_i}$
  \]
- The activation just checks if ${\(z \geq 0\)}$.  

# Application of a Perceptron
1. A perceptron takes inputs, multiplies them by weights, adds them up, and compares the result to a threshold.  
2. If the weights (**wᵢ**) are known, we can use the perceptron to classify objects.  
3. If we can *learn* the weights from data, the perceptron becomes a machine learning model.  
4. Before learning, we must ask: even with the right weights, can the perceptron solve the classification task (e.g., decide if a number is positive)?

Can a perceptron represent a logic gate?

NOT - Takes in input and reverses it : 0 -> 1, 1 -> 0
AND


# Training a Perceptron
A supervised algorithm analyzes the training data and produces an inferred function which can be used for mapping new examples.
It looks at the examples and updates and adjusts the weights so that its predictions start matching known answers, and over time it learns a rule to classify unseen data.

Going through each iteration, the weights are originally a low value. Once the perceptron has a prediction, we compare it to the correct answer and update the weights. We keep going until the weight updates are below a threshold - the perceptron has "converged".


# Adaline: Introduction
ADAptive LINear NEuron
A single-layer neural network developed by Widrow and Hoff at Stanford University
Perceptron limits - only works when classes trying to identify are linearly separable. 
	Stops when it finds a linear separation, but not necesarily an optimal one


Adaline - a single layer neural network
	Utilizes a differentiable cost function (measure of how good a fit the model is to the data on which it's being trained)

# Training Adaline Model
In perceptron, the activation function follows the sum of inputs * weights. In the adaline model, the value of the activation function is the input to the quantizer, and the output of the quantizer is the output of the entire model.
The weights are adjusted after the activation function, just as in the perceptron.

# Cost Function
