Perceptron is a one-layer neural network that can classify things into two parts
Inspired by neurons that receive electrical signals from other neurons - depending on strength can activate or stay put

Artificial neuron takes input values multiplied by weights - sums them up with a static number (bias) - passes them through activation function that maps them to an output - 0 if not activated, 1 if activated

A single unit of logic in an artificial neural network
An algorithm that outputs a binary conclusion

Example:
Thoughts are a response to a sensory input
Tasting food, food is input, the input goes to sense and then make a conclusion whether you like it or not
Just because you like something, doesn't mean someone else will

Perceptrons can come to different conclusions

Main component is input data

Input: X = {0.1, 0.5, 0.2}
Weights: W = {0.4, 0.3, 0.6}
X_1 = 0.1, W_1 = 0.4, etc
Inputs are multiplied by weights
Adding results to get a weighted sum, see if it reaches a threshold (called bias)

Is the sum > threshold?

To evaluate if it was reached, an activation function is used. Simplest is a step function. If the weighted sum is less than or equal to threshold, returns 0. If greater, than it returns 1. 
We choose the threshold/bias value, and we choose the initial weights.

```python
x_input = [0.1, 0.5, 0.2]
w_weights = [0.4, 0.3, 0.6]
threshold = 0.5

def step(weighted_sum):
	if weighted_sum > threshold:
		return 1
	else:
		return 0

def perceptron():
	weighted_sum = 0
	
	for x,w in zip(x_input, w_weights):
		weighted_sum += x * w
		print(weighted_sum)
	
	return step(weighted_sum)

output = perceptron()
print(f"Output: {output}")
```

