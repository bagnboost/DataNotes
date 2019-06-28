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
4.  If the total error rate is the smallest error rate so far, 



    


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMDE0NDc4ODJdfQ==
-->