# 8/18
## Welcome to Machine Learning
Examples of uses of Machine Learning:
	-Voice to text
	-Streaming service recommendations
	-Ranking web pages
Machine Learning is helping in other areas such as healthcare, climate change. 
Machine learning is the science of getting computers to learning without explicit programming. 

## Applications of Machine Learning
Desire to build intelligent machines. Algorithms helped with things like finding shortest paths, but for things like self-driving cars, only having machines learn could those concepts be reached. 
Artificial General Intelligence: someday building machines as intelligent as a human. Still a long way from that goal. The best way to get closer is by using learning algorithms that might take inspiration from how the human brain works. 

## What is Machine Learning?
"Field of study that gives computers the ability to learn without being explicitly programmed". -Arthur Samuel (1959)
Programmed a checker's program that played against itself tens of thousands of times and learn the best way to play checkers and eventually became a better checker's player than Arthur Samuel. 
Main types of machine learning algorithms
-Supervised Learning: used most in real-world applications, has rapid advancements. 
-Unsupervised Learning
-Recommender systems
-Reinforcement learning

## Supervised Learning part 1
Most economic value provided by ML is from supervised learning
Algorithms that learning x->y, or input to output 
It learns from being given "right answers" and by seeing correct pairs of input x and output y, and the algorithm learns to just take the input alone without the output and gives a reasonably accurate prediction or guess of the output. 
Ex:
input x: email
input y: spam (y/n)
Application: spam filtering

input x: English
input y: text transcripts
Application: speech recognition

input x: English
input y: Spanish
application: machine translation

The most prevalent form of this today might be advertising. The input is an ad, or user info, the output is if the user clicked. 
For a self-driving car, the input would be an image, or radar info, and try to output the position of other cars.
For all of these applications, you first train it with the input and show the correct output. Then to test, show it a brand new input and have it try to output the correct answer.

Regression: predicting a continuous numerical value based on one or more independent features
For example: predicting housing prices

## Supervised Learning Part 2
Classification Algorithm
Take breast cancer detection in ML - building a tool that can take early breast cancer detection. It takes a tumor of any size and labels it as malignant or benign. You can plot the data where the x axis is the tumor size and the y output is malignant (0) or benign (1). This differs from regression in that the output is smaller - regression can be any number. In this example, there are only two outputs. It can be plotted on a line where an O is benign, an X is malignant.
In classification, classes and categories are used interchangeably. 
Classification Algorithms predict categories - can predict whether a picture is a cat or a dog. 
There can be more than one input to predict an output. 
For example, instead of just using tumor size, the input can also include the age. 