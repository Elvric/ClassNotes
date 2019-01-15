# COMD 551
# Lecture 1
### Evalutation
- 10% weekly quizzes
- 60% project/assignments
- 30% midterm

### Quizzes every week
2 quizzes on Tuesday from 12am to 11pm the other one one friday. They will then take the best grade out of the two 


## Understand data sets
Columns of the data sets are called: input variables, features or attributes (information given to the algorithm to make predicition about the input variables)

Columns that we are predicted are called output variables or targets

Rows in the table are called of training example or instance

The entire table is called the data set

# 10/01/19
## Supervised learning
We are given a set of training examples (table row) we denote that:
$$ x_i, x_{ij}$$
where  is is the j fearure of the ith examples yi will be the disired output.

Assumption: i.i.d each example given to train is independent of any other row (no correlation between the data points).
- independent
- identical: the distribution is the same for all examples

## Predicition problems
### Classification
is a treatment yes or no predicting 0 or 1 basically

### Regression
The output is a real number (predicting the change in size) then the output is a numerical values.

#### Empirical risk minimization
For a given function class F and training sample S. To do that we define a notion of error Ls(f) = # mistake made on sample S  
From there we define a ERM (Emerical Risk Minimization):

#### Features
Can be numerical  
categorical (from a discret set span, not spam, rough smooth)  
Ordinal: given a ranking withouth any metric relation

## Linear models
$$ f_w(x) = w_0 + w_1x + w_2x^2$$
The w are called param or weights, the w0 allow us to shift the function up or down.

### Least Squares solution method
Goal is to find the best linear model given a data using specific evaluation criteria here the error is
$$ Err(w) = \sum_{i = 1:n}(y_i-w^Tx_i)^2$$
Here the T means transpose

### Robbins-Monrow conditions
Ensure that the linear regression function converges to a minimum at some point
$$ \sum_{k=0:\infty} \alpha_k = \infty\\
\sum_{k=0:\infty} \alpha_k^2 < \infty$$


### Convex functions
Were there is only one global min which ensure that we always find the min.  
Lost landscape means that the function looks very wavy which makes the gradient decent method super jumpy in terms of results overtime.

# 15/01/19
### Limitation of linear regression
The case were X^TX where the matrix is singular with 0 eigen values then its inverse is undefind.  
A simple lienar function is a bad fit where the data is so scatered that a linear fit will not be a good enough model.

When a matrix is singular in our case is that one of the feature can be predicted by other features (2 features are corrolated) the matrix must be full column rank.

The number of features/weights exceeds the number of training examples, reduce the number of features using later seen technique.

## Inputs variables for linear regressions
Before we had X1..Xm but there exists other methods:
- Xm+1 = log(X1)
- Xm+1 = Xi^2, Xm+2 = Xi^3
- XiXj
- qualitative variables Xm+1 = 1 when true otherwise 0.

Then we can add these new variables to our model.

What if a variable represents 4 colors
- 4 separate binary variables (issue is that these features are correlated)
- 3 separate binary variables instead where the last color is the absence of true values.

## Overfitting
We can generate models that fit perfectly the trainging data but will not be great at predicting the new data. The generalization is not that great. This is because it matches the data exactly but is wilde everywhere else.

In order to check for overfitting, we measure error over a training set. If f1 has lower error on training set than f2 but has higher error on true error test (new data) than f2 then f1 is over fitting.

What really matters is performing well on new data this is the goal generilization. A good debugging is to try to fit the training data as much as possible see if this is right and from there try to generate the generialzed model.

### Evaluating on held out data
partition the data into:
- Training set
- validation set
- test set

#### Training set
Is used to train the model (find the best hypo)

#### Validation set
used for model selection to estimate true error and compare the hypo classes

#### Test set
What is the final accuracy of our model used to write the performance report.

### k-fold cross validation
We call each following even a fold
1. k partitions of the training data (equal size usually). 
2. train with k-1 subsets validate on the k subset (repeat k times)
3. Average the prediction error over the k folds

**This increases the computation time by a factor of k.

### Leave one out cross validation
Set asside one data point from the training set so if we have n data points then we repeat that n times

Average the perdiction error over all n subsets. And choose the setting with the lowest estimated true prediction error.

This is good when the data set is small but if the model is 1 000 000 with 10 000 features this model will take too much time.