## Decision Tree Builder

### Categorical Features

#### Define features

	FEATURES = ['x1', 'x2','x3','x4',...]  

2. All categorical features are converted to binary variables

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

for feature in features features:  
 data_one_hot_encoded=data[feature].apply(lambda x:{x:1}) 
    


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3OTM0ODQ3NjNdfQ==
-->