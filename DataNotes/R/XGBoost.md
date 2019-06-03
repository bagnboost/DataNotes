## Load library
library(xgboost)

## Set the training and validation sets
    xgtrain <- xgb.DMatrix(data = data.matrix(df_training[,]),
			               label = df_training[,"response_variable"]))

    xgvalid <- xgb.DMatrix(data = data.matrix(df_validation[,]),
			               label = df_validation[,"response_variable"]))

## Set the watchlist
These are the datasets we want evaluated by the model, like xgtrain and xgvalid.

    watchlist <- list(train = xgtrain,
                      validation = xgvalid)

## Train the model
    xgb_model <- xgb.train(data = xgtrain,
    				       objective = "binary:logistic",
    				       watchlist = watchlist,
    				       eval_metric="auc",
    				       booster = "gbtree",
                           nthread = 4,
                           seed = 0,
                           max_depth = 5,
                           eta = 0.0001,
                           nrounds = 100,
                           alpha = 0.1,
                           min_child_weight = 2,
                           subsample = 0.8
                          )
## Hyperparameters
 - objective
	 - reg:linear (default)
	 - binary:logistic (returns probability)
	 - multi:softmax, for multiclass classification (returns class)
	 - multi:softprob, for multiclass classification (returns probability of each class)
 
 - eval_metric
	 - Default values are rmse for regression and error for classification
	 - Values include
		 - rmse: root mean square error
		 - mae: mean absolute error
		 - logloss: negative log-likelihood
		 - error: binary classification error rate (0.5 threshold)
		 - merror: multiclass classification error rate
		 - mlogloss: multiclass logloss
		 - auc: area under the roc curve
 
 - booster
	 - gbtree for tree-based models (default)
	 - gblinear for linear models

- seed
	- Random number seed
	- Default = 0

- max_depth
	- The maximum depth of the three
	- Default = 6, typical values are 3 to 10

- max_leaf_nodes
	- The maximum number of terminal nodes or leaves in a tree.
	- This is an alternative way of specifying depth, since a tree of depth "n" would produce 2^n leaf nodes.
	- Don't specify both max_depth and max_leaf_nodes.

- eta
	- Learning rate
	- Default = 0.3
	- Typucal values: .01 - 0.2

- nrounds
	- Number of iterations through the data

- alpha
	- L1 regularization term (Ridge Regression)
	- Default = 0

- min_child_weight
	- The minimum sum of weights of all observations required in a child
	- Higher values prevent overfitting, but too high values can produce underfitting
	- Default = 1

- subsample
	- Fraction of observations to be randomly sampled for each tree
	- Lower values make the model more conservative and prevents overfitting. but too small values can lead to underfitting.
	- Default = 1
	- Typical values: 0.5 - 1.0

- num_class
	- Specify the number of classes in classification problem, if more than 2

## Variable Importance
Calculate the importance of each feature.

    xgb_importance <- xgb.importance(model = xgb_model)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExOTc5NTc0NTNdfQ==
-->