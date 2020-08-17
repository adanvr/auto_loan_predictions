Started exploratory analysis by investigating structure of data including column types and distributions. Decided to exclude any columns with a high degree of missing values - this was somewhat arbitrarily chosen to be anything greater than 20%. This tolerance should be set by client or primary stakeholder as there may be a reason for the high degree of missing; alternatively, the missingness could also be informative, perhaps it represents some value that would best be re-coded as something other than NULL or NaN.
Then I checked distribution of numeric features by doing individual boxplots as when I tried to graph all of the boxplots in a single graph the loan_amount variable was on a much different scale than the other features. Records containing "outlier" values for numeric features defined as being outside the IQR of a boxplots were excluded from the pipeline. This should, again, be something determined by the stakeholder. Missing values were then re-coded as "missing" rather than left as NaN since they are commonly representative of the data. I then manually re-coded the categorical values by looking at the values in each column and making some educated guesses as to whether they were ordinal and whether that ordering could be something I could discern as someone without the subject matter expertise of a stakeholder. I then processed the features by scaling the numeric ones (i.e. integers and floats) and one-hot encoding the categorical features. 
I then split up the data into train/validation/split. I used the training data to compare different preprocessing methodologies (e.g. SMOTE for class imbalance of the outcome) as well as different algorithms and hyperparameter tuning sets. I compared an elastic net penalized logistic regression model to a random forests tuned over a pre-defined set of hyperparameters with and without the use of SMOTE. The models were tuned using 5-fold cross validation. I found that the best performing model was a l2 penalized logistic regression model without the use of SMOTE to account for class imbalance of the y target variable. The AUC score on validation was 0.65 and the AUC score on the test set was 0. The auc score on the test set was 0.53 indicating that the model is underfitting. 
I chose to use these two models as they are common "simpler" models that are more "explainable" than more complex algorithms such as deep learning neural networks or boosted random forest algorithms. I chose to use these models for this exercise as I thought the clients might be interested in better understanding what features are predictive of a new customer defaulting on their loan. It is also useful that these models output are considered "soft-classification" algorithms that output a predicted probality rather than a "hard classification" ML algorithm that would only output a predicted class label. This allows the client to set a thresshold value for the decision boundary between classifiying a new customer as either "likely to default" (i.e. risk = 'bad') or "not likely" (ie. risk = 'good'). This also allows the user to quantify the level of risk associated with this potential customer by comparing the predicted probability of an individual new customer to some reference level (e.g. aggregated risk of a reference population or benchmark) so that you can make statements such as "this customer is 10% more likely to default on their new loan than this other population".  I would enable an end-user to modify this algorithm based on their own risk tolerances by allowing them to input their own optimal_cutoff value. I empirically found this to be ~ 0.28 by finding the thresshold that maximized the difference between the true positive rate and false positive rate of the training data. However, a customer or client might have their own reasons for choosing a different cutoff. Presumably there is some cost associated with misclassification error (e.g. false positives or false negatives). So by enabling the end user to customize that decision boundary based on subject mattery expertise it would empower them to best assess how they would like to proceed.
I would reccomend not enabling this model for a bank to make such important decisions as the test auc is barely out-performing a naive model of say flipping a coin with an auc of 0.5. I would also recommend to the client to see if there are other sources of information that we could incorporate to help improve performance (e.g. historical financial records or transactions). Perhaps there is a relationship between other loans (e.g. home or small business) that are also predictive of a customer defaulting on their loan.