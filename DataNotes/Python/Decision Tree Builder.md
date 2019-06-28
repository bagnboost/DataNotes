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

#### Calculating the split errors
We first need to define the method for calculating the errors at each split so we can determine the best feature to split on.

    def split_errors(split_responses):
	    #If the responses are empty,
	    #then all of the responses belong to one class
	    #so there are no mistakes
	    if (len(split_responses) == 0):
		    return 0
		    
		#If the above condition is not met,
		#there is some error
		#Count the number of responses equal to 0
		data_response_0 = len(split_responses[split_responses == 0])
		#Count the number of responses equal to 1
		data_response_1 = len(split_responses[split_responses == 1])
		
		#Calculate the number of errors
		#Whichever class is less represented are the errors
		if (data_response_0 >= data_response_1):
			errors = data_response_1
		elif (data_response_1 > data_response_0):
			errors = data_response_0
		
		return errors
	    

#### Determining the best split
Now that the metric for best split is defined, we can calculate it one feature at a time and determine the best feature to split on.

This function's purpose is just to find the best feature to split the current node. It says nothing about what to do after the best feature is found.

    def best_split(data, features, response):
	    #Initialize the best feature for storing
	    best_feature = None
	    #Initialze the best error rate for storing
	    #Error rates are always <= 1, so we choose something larger
	    best_error_rate = 2

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
			
			#Calculate the error rate of this split
			error_rate = (left_errors + right_errors)/total_observations)
			
			#If this is the lowest error rate found so far,
			#store the feature anf error rate
			if error_rate < best_error_rate:
				best_feature = feature
				best_error_rate = error_rate

#### Creating the leaf nodes
We have our best split metric and the algorithm for determining which feature provides the best split at a single node.

Before creating the entire tree, we want to also define the stopping point - the leaf nodes.

    def create_leaf(split_responses):
	    #Create a leaf dictionary
	    leaf = {'splitting_feature]: None,
				'left' : None,
				'right': None,
				'is_leaf: True}
		
		#Count the total obseravations of each class
		class_zero = len(split_responses[split_responses == 0])
		class_one = len(split_responses[split_responses == 1])

	    #The prediction for theleaf will be the majority class
	    if (class_zero >= class_one):
		    leaf['prediction'] = 0
		elif (class_one > class_zero):
			leaf['prediction'] = 1
		
		return leaf

#### Build the tree
We will now introduce another stopping criteria called the max depth, which will prevent the tree from creating too many intermediate no

    def build_tree(data, features, response, current_depth = 0, max_depth = 10):


    


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMTkzMjY0NjVdfQ==
-->