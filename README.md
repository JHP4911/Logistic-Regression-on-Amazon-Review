# Logistic Regression on Amazon Reviews (Part I) #

## Amazon Fine Food Review Dataset ##

Data Source: https://www.kaggle.com/snap/amazon-fine-food-reviews

The Amazon Fine Food Reviews dataset consists of reviews of fine foods from Amazon. <br/>
Number of reviews                   : 568,454  <br/>
Number of users                     : 256,059  <br/>
Number of products                  : 74,258  <br/>
Timespan: Oct 1999                  : Oct 2012  <br/>
Number of Attributes/Columns in data: 10 <br/>

### Attribute Information ###
1. Id <br/>
2. ProductId - unique identifier for the product <br/>
3. UserId - unqiue identifier for the user <br/>
4. ProfileName <br/>
5. HelpfulnessNumerator - number of users who found the review helpful <br/>
6. HelpfulnessDenominator - number of users who indicated whether they found the review helpful or not <br/>
7. Score - rating between 1 and 5 <br/>
8. Time - timestamp for the review <br/>
9. Summary - brief summary of the review <br/>
10. Text - text of the review <br/>

## Objective ##

The code below would **clean the review text from html tags and punctuations and write it as a new column in the database and write it to disk**. This is further taken up in Part 2 to find accuracy of 10-fold cross validation KNN on vectorized input data, for each of the 4 featurizations, namely BoW, tf-IDF, W2V, tf-IDF weighted W2V.

## Significant Points ##

1. **Duplication of reviews** are found with same userid and timestamp (Cleaned).
2. Found discrepancy issues with HelpfulnessDenominator (Cleaned).
3. final.sqlite db is to be **used for further processing** such as Text to Vector operations.
4. The preprocessing step is one time effort but the training & visualization steps require multiple runs. Hence, it is prudent to make reprocessing step independant, to avoid multiple runs.

# Logistic Regression on Amazon Reviews Dataset (Part II) #

## Data Source ##

The preprocessing step has produced final.sqlite file after doing the data preparation & cleaning. The review text is now devoid of punctuations, HTML markups and stop words.

## Objective ##

**To find optimal lambda using GridSearchCV & RandomSearchCV** on standardized feature vectors obtained from BoW, tf-idf, W2V and tf-idf weighted W2V featurizations. To study the impact on sparsity upon increasing lambda.

**Find Precision, Recall, F1 Score, Confusion Matrix, Accuracy of 10-fold cross validation with GridSearch and RandomSearch with optimal Logistic Regression regression model on vectorized input data, for BoW, tf-idf, W2V and tf-idf weighted W2V featurizations**. TPR, TNR,
FPR and FNR is calculated for all.

After finding the optimal model, **do Perturbation test** to remove multicollinear features. **Find top n words** using the weight vector, w.

## At a glance ##

Random Sampling is done to reduce input data size and time based slicing to split into training and testing data. The optimal lambda is found out using GridSearchCV & RandomSearchCV with a range of lamda values to search (for GridSearch) and an uniform distribution (for RandomSearchCV.

The Precision, Recall, F1 Score, Confusion Matrix, Accuracy metrics are found out for all 4 featurizations. A normal distribution noise is added for perturbnatino test and the identified multicollinear features are removed. Then the top ???n??? words are found out after removal of multicollinear features based on highest values of |w|.

## Custom Defined Functions ##

5 user defined functions are written to
1. Perform GridSearchCV & RandomSearchCV for Optimal Alpha Estimation.
2. Compute Logistic Regression Classifier Performance Metrics.
3. Find Most Frequent Words.
4. Analyze Sparsity for increasing Lambda.
5. Perturbation Test with a Normal Distributed Noise. <br/> Sparsity of input vector is preserved for BoW and tf-idf featurizations. For W2V and tf-idf W2V the features are dense.

## BoW ##

BoW will result in a sparse matrix with huge number of features as it creates a feature for each unique word in the review.

For Binary BoW feature representation, CountVectorizer is declared as float, as the values can take non-integer values on further processing. Top n words are found out after checking for multicollinearity.

![rscv](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/cm_rscv.PNG)

![gscv](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/cm_gscv.PNG)

## Sparsity vs F1 score Plot ##

The variation of sparsity corresponding to varying values of lambda is plotted and the lambda with the highest accuracy is identified. The optimal model can be found out using the sparsity vs f1 score plot also.

![spar](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/sp.PNG)

![score](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/scorep.PNG)

## tf-IDF ##

Sparse matrix generated from tf-IDF is fed in to GridSearch and RandomSearch Logistic Regression Cross Validator to find the optimal lambda value. Performance metrics of optimal LR with tf-idf featurization is found.

The optimal value of lambda using RandomizedSearchCV is 0.002245.

**Metric Analysis of Logistic Classifier for Optimal Lamdba** <br/>
Accuracy = 86.033333 <br/>
Precision = 91.228756 <br/>
Recall = 92.310733 <br/>
F1 Score = 91.766555 <br/>

**Confusion Matrix** <br/>
True Negatives = 492 <br/>
True Positives = 4670 <br/>
False Negatives = 389 <br/>
False Positives = 449 <br/>

Total Actual Positives = 5059 <br/>
Total Actual Negatives = 941 <br/>
True Positive Rate(TPR) = 0.92 <br/>
True Negative Rate(TNR) = 0.52 <br/>
False Positive Rate(FPR) = 0.48 <br/>
False Negative Rate(FNR) = 0.08 <br/>

**GridSearchCV: Best C: 0.001**

The optimal value of lambda using GridSearchCV is 1000.000000.

**Metric Analysis of Logistic Classifier for Optimal Lamdba** <br/>
Accuracy = 89.283333 <br/>
Precision = 90.000000 <br/>
Recall = 98.201226 <br/>
F1 Score = 93.921921 <br/>

**Confusion Matrix** <br/>
True Negatives = 389 <br/>
True Positives = 4968 <br/>
False Negatives = 91 <br/>
False Positives = 552 <br/>

Total Actual Positives = 5059 <br/>
Total Actual Negatives = 941 <br/>
True Positive Rate(TPR) = 0.98 <br/>
True Negative Rate(TNR) = 0.41 <br/>
False Positive Rate(FPR) = 0.59 <br/>
False Negative Rate(FNR) = 0.02 <br/>

**Length of Weight Vector (Before Removing Collinearity): 15114** <br/>
Distance between Weight vectors before & after Perturbation = 2.93 <br/>
Multicollinear Features = 7557 <br/>

**Length of Weight Vector (After Removing Collinearity): 7557**

![rscv2](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/cm_rscv2.PNG)

![gscv2](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/cm_gscv2.PNG)

## Word2Vec ##

Dense matrix generated from Word2Vec is fed in to GridSearch and RandomSearch Logistic Regression Cross Validator to find the optimal lambda value.

Performance metrics of optimal LR with W2V featurization is found. But we cannot find the top ???n??? words when we use Word2Vec based featurization, because the feature doesnt correspond to a word in the vocabulary.

**The optimal value of lambda using RandomizedSearchCV is 103.277877.**

**Metric Analysis of Logistic Classifier for Optimal Lamdba** <br/>
Accuracy = 84.250000 <br/>
Precision = 84.386493 <br/>
Recall = 99.782566 <br/>
F1 Score = 91.440993 <br/>

**Confusion Matrix** <br/>
True Negatives = 7 <br/>
True Positives = 5048 <br/>
False Negatives = 11 <br/>
False Positives = 934 <br/>

Total Actual Positives = 5059 <br/>
Total Actual Negatives = 941 <br/>
True Positive Rate(TPR) = 1.0 <br/>
True Negative Rate(TNR) = 0.01 <br/>
False Positive Rate(FPR) = 0.99 <br/>
False Negative Rate(FNR) = 0.0 <br/>

**GridSearchCV: Best C: 100**

The optimal value of lambda using GridSearchCV is 0.010000.

**Metric Analysis of Logistic Classifier for Optimal Lamdba** <br/>
Accuracy = 52.150000 <br/>
Precision = 86.515354 <br/>
Recall = 51.235422 <br/>
F1 Score = 64.357542 <br/>

**Confusion Matrix** <br/>
True Negatives = 537 <br/>
True Positives = 2592 <br/>
False Negatives = 2467 <br/>
False Positives = 404 <br/>

Total Actual Positives = 5059 <br/>
Total Actual Negatives = 941 <br/>

True Positive Rate(TPR) = 0.51 <br/>
True Negative Rate(TNR) = 0.57 <br/>
False Positive Rate(FPR) = 0.43 <br/>
False Negative Rate(FNR) = 0.49 <br/>

**Length of Weight Vector (Before Removing Collinearity): 300** <br/>
Distance between Weight vectors before & after Perturbation = 0.0 <br/>
Multicollinear Features = 9 <br/>

**Length of Weight Vector (After Removing Collinearity): 291**

![rscv3](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/cm_rscv3.PNG)

![gscv3](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/cm_gscv3.PNG)

## TF-IDWeighted W2V ##

Grid Search and Random Search CV using Logistic Regression Best Penalty: l1 <br/>
RandomizedSearchCV: Best C: 0.9648400471483856

The optimal value of lambda using RandomizedSearchCV is 1.036441.

**Metric Analysis of Logistic Classifier for Optimal Lamdba** <br/>
Accuracy = 52.566667 <br/>
Precision = 83.540467 <br/>
Recall = 54.477169 <br/>
F1 Score = 65.948792 <br/>

**Confusion Matrix** <br/>
True Negatives = 398 <br/>
True Positives = 2756 <br/>
False Negatives = 2303 <br/>
False Positives = 543 <br/>

Total Actual Positives = 5059 <br/>
Total Actual Negatives = 941 <br/>

True Positive Rate(TPR) = 0.54 <br/>
True Negative Rate(TNR) = 0.42 <br/>
False Positive Rate(FPR) = 0.58 <br/>
False Negative Rate(FNR) = 0.46 <br/>

GridSearchCV: Best C: 100

**The optimal value of lambda using GridSearchCV is 0.010000.**

**Metric Analysis of Logistic Classifier for Optimal Lamdba** <br/>
Accuracy = 50.833333 <br/>
Precision = 86.424870 <br/>
Recall = 49.456414 <br/>
F1 Score = 62.911743 <br/>

**Confusion Matrix** <br/>
True Negatives = 548 <br/>
True Positives = 2502 <br/>
False Negatives = 2557 <br/>
False Positives = 393 <br/>

Total Actual Positives = 5059 <br/>
Total Actual Negatives = 941 <br/>

True Positive Rate(TPR) = 0.49 <br/>
True Negative Rate(TNR) = 0.58 <br/>
False Positive Rate(FPR) = 0.42 <br/>
False Negative Rate(FNR) = 0.51 <br/>

**Length of Weight Vector (Before Removing Collinearity): 300** <br/>
Distance between Weight vectors before & after Perturbation = 12.02 <br/>
Multicollinear Features = 136 <br/>

**Length of Weight Vector (After Removing Collinearity): 164** <br/>

![rscv4](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/cm_rscv4.PNG)

![gscv4](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/cm_gscv4.PNG)

## Summary Statistics ##

![sum](https://github.com/AdroitAnandAI/Logistic-Regression-on-Amazon-Reviews/blob/master/images/summary.PNG)

## Observations ##

1. From the Sparsity and F1 Score plot, it can be identified that **Performance & Sparsity is the best when Log (Lambda) is between 1 and 2**. i.e. Lambda = 10??1 ~ 10??2 = 10 ~ 100. The lambda values obtained via plotting method is almost same as the lambda value found
out by GridSearchCV and RandomSearchCV. (Please note that, Sparsity = # of non-zero elements, in this project).

2. It has also been noticed that, **with increasing lambda, the sparsity (# of non-zero elements) has been decreasing steadily**. This is an expected behaviour, as **L1 regularization** is used.

3. The Lambda values found by GridSearchCV and RandomizedSearchCV are near, only when the range of "C" values is set within a narrow range, around optimum. i.e. if the optimal C = 1 (as per GridSearchCV), then by setting C as a uniform distribution between 0 and 4
will yield C = 1 (+/- 0.05) approximately, within say, 100 iterations. But if C value is set as a uniform distribution between 0 and say, 10000, then the error in C value is found to be very high.

4. Alternatively, **if the range of C value is wide, to arrive at optimal C, we need to increase the number of iterations** significantly. It is seen that, when iterations are increased from 100 to 1000, the C value is converging to optimum. But the **time complexity of such an approach would be much higher**.

5. Because of 3 and 4, it is suggested to **use GridSearchCV for faster convergence when the number of dimensions are less**. But, when the # of hyperparameters increase, the # of times the model needs to be trained, increases exponentially. If there are k hyperparameters, then m??k trainings would be required. Hence, **grid search is not good when hyperparameters are more**. In Logistic Regression, there could be only 2 hyperparameters. But there are cases in deep learning where there are 10s or 100s of hyper parameters.

6. **Random Search** is almost as good as Grid search, and also **faster than Grid search when # of hyper parameters is large**. But since the number of iterations required to find the optimal lambda for multiple dimensions is much more, more processing power may be required. Still, it would perform better than the exponential time requirement of Grid Search.

7. The elements of **W2V vector doesnt correspond to each word feature, like in the BoW vector or TF-ID vector**. Hence the weight vector w, that you get, which would be of the same length as W2V vector, once you fit logistic regression, doesnt correlate to word features. **Hence, we cannot find the top ???n??? words when we use Word2Vec based featurization**. But we can still find the top ???n??? features based on the weight vector, but that do not correspond to any word, hence not interpretable.

8. The best method is found to be **Logistic Regression on Bag of Words**. This method has the highest F1 Score, amongst all the 4 methods. Hence, Bag of Words featurization with Logistic Regression is the classifer of choice.
