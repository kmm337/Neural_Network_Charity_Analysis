# Neural_Network_Charity_Analysis

## Overview

The goal is to help the foundation predict where to make investments. 

This new assignment consists of three technical analysis deliverables and a written report:
* Deliverable 1: Preprocessing Data for a Neural Network Model
* Deliverable 2: Compile, Train, and Evaluate the Model
* Deliverable 3: Optimize the Model
* Deliverable 4: A Written Report on the Neural Network Model (README.md)

## Data Preprocessing

* Target variable: Whether or not the application was successful.
* Feature Variables: APPLICATION_TYPE, AFFILIATION, CLASSIFICATION, USE_CASE, ORGANIZATION, STATUS, INCOME_AMT, SPECIAL_CONSIDERATIONS, ASK_AMT 
* Variables to remove:  EIN and NAME

## Compiling, Training, and Evaluating the Model

* How many neurons, layers, and activation functions did you select for your neural network model, and why? My final model had 2 hidden layers, each with 2 nodes. The output layer had 1 node. I used the tanh activation function in the hidden layers because the data was scaled with center at zero and tanh allows negative values.
* Were you able to achieve the target model performance? No.
* What steps did you take to try and increase model performance? I found stratifying the sample, changing the activation function from relu to tanh, and reducing the number of nodes improved the fit and accuracy.

## Steps in Optimization:

Run 1:

model: nodes 80,30. Accuracy 72.6%, Loss 55.38%, validation split=.2
stratify=False, epochs=100

Results and analysis:

underfitting: Does not seem to be a problem, the loss curve for the training dataset shows a declining curve seeming to converge.
overfitting: Does seem to be an issue, the loss curve for the validation dataset declines a bit and then starts increasing.
Representativeness: the training dataset shows improvement in loss but the validation dataset shows virtually no improvement, the loss curve is noisy and tending upward. There is a large gap between the datasets. 

Train dataset = 20,579
Validation dataset = 4,116
Test dataset = 8,575

The dependent variable, success, is approximately evenly split between successul and not successful (18,261 and 16,038, respectively).

Next steps: Use stratified sampling to ensure successful/unsuccessful applications are evenly split between the train, validation and test datasets.

Run 2:

model: nodes 80,30. Accuracy 72.6%, Loss 55.38%, validation split=.2
stratify=True, epochs=100

Results and analysis:

The gap in the loss curves between the training and validation datasets is greatly reduced, suggesting stratified sampling has resolved the representativeness issue. The increasing loss in the validation dataset suggests overfitting or that the validation dataset is too small.

Next steps: Increase the validation sample from .2 to .3.

run 3:
model: nodes 80,30. Accuracy 72.9%, Loss 55.68%, validation split=.3
stratify=True, epochs=50

Results and analysis:

Increasing the size of the validation dataset does not help.

Next steps: Look for noise in the input variables. 

Run 4:
model: nodes 80,30. Accuracy 72.98%, Loss 56.63%, validation split=.2
stratify=True, epochs=100, ASK_AMT transformed into bins

Evaluating all the feature variables, it is clear that ASK_AMT is severely right-skewed. Of the 33,270 applications, 76% are for exactly $5,000. The larger ASK_AMTs are between $15,583 and almost $9 billion. I looked at square root, cube root and natural log transformations but they could not reduce the skewness significantly. I created 4 bins:

Results and analysis:

Binning the ASK_AMT variables does not seem to have improved the situation. Reduce the number of epochs to address overfitting.

Run 5:

model: nodes 80,30. Accuracy 72.98%, Loss 56.63%, validation split=.2
stratify=True, epochs=50, ASK_AMT transformed into bins

Results and analysis:

Again, not much improvement. Change the random_state variable in train_test_split to see if there's an issue with the sampling.

Run 6:

model: nodes 80,30. Accuracy 72.98%, Loss 56.63%, validation split=.2
stratify=True, epochs=50, ASK_AMT transformed into bins, random_state=25

Results and analysis:

Again, not much improvement. Look at model structure to reduce overfitting. Reduce the number of neurons in the first hidden layer to 20.

Run 7:
Reduced nodes in first hidden layer to 50. No change.
Reduced nodes in second hidden layer to 20. No change.
Added a third hidden layer wtih 10 nodes. No change.

Try changing the activation function in the hidden layers. Since the data is scaled with mean 0, the ReLU activation function gives any value below zero, the value zero. Try Tanh which is zero-centric, or ELU

Run 8:

model: nodes 80,30. Accuracy 72.61%, Loss 55.58%, validation split=.2
stratify=True, epochs=50, ASK_AMT transformed into bins, random_state=78

Results: validation sample is closer to train sample, but still looks like the validation sample is not well represented by the training sample. Increase validation to .3

Run 9
model: nodes 80,30. Accuracy 72.61%, Loss 55.58%, validation split=.3
stratify=True, epochs=50, ASK_AMT transformed into bins, random_state=78

Run 10
try activation function  ELU

Look at more variables:
STATUS has only 5 values with 0, so drop
SPECIAL_CONSIDERATIONS ha only 27 values with Y, drop

Run 11
model: nodes 80,30. Accuracy 73.06%, Loss 55.73%, validation split=.3
stratify=True, epochs=50, ASK_AMT transformed into bins, random_state=78,
dropped STATUS, activation function=tanh

Run 12
model: nodes 80,30. Accuracy 72.61%, Loss 55.58%, validation split=.3
stratify=True, epochs=50, ASK_AMT transformed into bins, random_state=78,
dropped STATUS, SPECIAL_CONSIDERATIONS, activation function=tanh

Reduce the number of nodes per layer but increase layers
Run 13
nodes: 30, 10, 5

Run 14
nodes 15, 8, 3

Run 15
nodes 10, 5, 3

Run 16
nodes 6, 3, 2

Run 17
drop epochs to 30

Run 18
drop third layer

Run 19
nodes: 4, 2

Run 20
nodes: 3, 2

Run 21
nodes: 2,2

Run 22
reduce to 10 epochs

Run 23
drop second layer

Run 21 showed great fit and could reduce epochs

20 epochs accuracy 72.93