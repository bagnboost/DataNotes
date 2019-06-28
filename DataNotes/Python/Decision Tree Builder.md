## Decision Tree Builder

### Categorical Features

#### Define categorical features

	categorical_features = ['x1', 'x2', 'x3', 'x4',...]
	data = data[categorical_features]  

#### All categorical features are converted to binary variables
    for feature in features categorical_features:
	    data_one_hot_encoded=data[feature].apply(lambda x:{x:1})
	    data_unpacked = data_one_hot_encoded.unpack(column_name_prefix = feature)
	
		#Fill NAs with 0s
		for column in data_unpacked.column_names():
			data_unpacked[column] = data_unpacked[column].fillna(0)
	
		#Remove the original features
		data.remove_column(feature)
		data.add_columns(data_unpacked)
	
#### Determine the split metric
There are a number of methods that we can use:

 - Classification error
 - Gini Index
 - Information gain
 - etc.

For now, we will use classification error as the total misclassifications in the split divided by the total observations in the split.

#### Algorithm
1. Loop over every feature
2. Within a feature loop, split the data in two. One where observations = 0 (left split), one where observations = 1 (right split).
3.  Calculate the total error rate from both splits
4.  If the total error rate is the smallest error rate so far, store the feature and the error rate.

#### Determining the best split

    def best_split(data, features, response):
	    #Initialize the best feature for storing
	    best_feature = None
	    #Initialze the best error rate for storing
	    #Error rates are always <= 1, so we choose something larger
	    best_error = 2

		total_observations = float(len(data))
		
		#Loop through each feature
		for feature in features:
		
			#Left splits are where the data = 0
			left_split = data[data[feature] == 0]
			#Right splits are where the data = 1
			right_split = data[data[feature] == 1]
			
			#Calculate the error rate of the left split
			left_errors = split_errors(left_split[target])
			#Calculate the error rate of the right split
			right_errors = split_errors(right_split[target])
			error_rate = left_errors + right_errors/
			



    


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2MjU4MzE0NF19
-->