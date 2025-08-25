Features: items passed into model to help predict class
classification: a category

What is Machine Learning?
A subdomain of comp sci that focuses on algorithms which help a computer learn from data without explicit programming
AI vs ML vs DAS
AI: Artificial Intelligence: area of comp sci where the goal is to enable computers and machines to perform human-like tasks and simulate human behavior
ML: Machine Learning a subset of AI that tries to solve a specific problem and make predicitions using data
Data Science: field that attempts to find patterns and draw insights from data (Might use ML)
They all overlap

Types of machine learning:
Supervised Learning: uses labeled inputs (meaning the input has a corresponding output label) to train models and learn outputs
Ex: 
	You have three pictures of a cat, a dog, and a lizard. All three pictures are labeled as such. To the computer, they are just pixels. 
Unsupervised Learning: uses unlabeled data to learn about patterns in data
Reinforcement Learning: agent learning in interactive environment based on rewards and penalties. 

In supervised learning, inputs go into the model and spit out some output. The inputs are called feature vector. 
Features:
	Qualitative: categorical data (finite number of categories or groups)
		Ex: countries/nationalities, gender, age groups (Baby, toddlers, etc).
			Nominal Data - no inherent order
			One Hot encoding: if it matches some category, make it a 1, else make it 0
			Ordinal Data: inherent order - ex in age groups, toddlers are older than babies, adults are older than teenagers. These can be numbered
	Quantitative - numerical valued data( cold be discrete (integers), or continuous (all real numbers))

In Supervised Learning:
	Classifications: predict discrete classes
		Multiclass: multiple classifications. Ex: (hot dog, ice cream, pizza), (orange, apple, pear)
		Binary: Two classifications: Ex: True/False, Cat/Dog, Spam/Not Spam
	Regression: predict continuous values - a number that is on some sort of scale
		Ex: price of a house, stock, temperature

Training Dataset
	input x: fed into the model
	Prediction makes some sort of prediction
	Compare prediction to the y, the true value. Make adjustments to the training dataset as needed
Validation Dataset
	Once adjustments are made, validation set is used as a reality check during/after training to ensure model can handle unseen data
Testing Dataset
	Test set is a final check to see how generalizable the chosen model is

Loss: a numerical value that quantifies how close the model prediction is to the true value
The difference between prediction and the actual label
L1 Loss
	loss = sum(| y_real - y_predicted | )
	The absolute value of the real value of y subtracting the predicted value of y
L2 Loss
	loss = sum((y_real - y_predicted)^2)
	Quadratic, if it is close the penalty is minimal. If it is off by a lot, the penalty is higher.
Binary Cross-Entropy Loss
	Loss = -1 / N * sum(y_real * log(y_predicted) + (1 - y_real) * log((1 - y_predicted))
	Loss decreases as performance gets better

Accuracy: 
	Say there is a model being trained to p