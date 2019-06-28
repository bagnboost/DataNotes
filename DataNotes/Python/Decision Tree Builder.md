## Decision Tree Builder

### Categorical Features

1. Define features

	    FEATURES = ['x1', 'x2','x3','x4',...]  

2. All categorical features are converted to binary variables
 FEATURES = ['x1', 'x2','x3','x4',...]  

for feature in features features:  
 data_one_hot_encoded=data[feature].apply(lambda x:{x:1}) 
    


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NDc1NDE0MTddfQ==
-->