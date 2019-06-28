# Decision Tree Builder | Categorical Features

## Define categorical features

	categorical_features = ['x1', 'x2', 'x3', 'x4',...]
	data = data[categorical_features]  

## All categorical features are converted to binary variables

    for feature in categorical_features:
	    data_one_hot_encoded=data[feature].apply(lambda x:{x:1})
	    data_unpacked = data_one_hot_encoded.unpack(column_name_prefix = feature)
	
		#Fill NAs with 0s
		for column in data_unpacked.column_names():
			data_unpacked[column] = data_unpacked[column].fillna(0)
	
		#Remove the original features
		data.remove_column(feature)
		data.add_columns(data_unpacked)
	
## Determine the split metric
There are a number of methods that we can use:

 - Classification error
 - Gini Index
 - Information gain
 - etc.

For now, we will use classification error as the total misclassifications in the split divided by the total observations in the split.

## Algorithm
1. Loop over every feature
2. Within a feature loop, split the data in two. One where observations = 0 (left split), one where observations = 1 (right split).
3.  Calculate the total error rate from both splits
4.  If the total error rate is the smallest error rate so far, store the feature and the error rate.

## Calculating the split errors
We first need to define the method for calculating the errors at each split so we can determine the best feature to split on.

    def split_errors(split_responses):
	    
	    #If the responses are empty,
	    #then all of the responses belong to one class
	    #Thus, there are no errors and we can return 0
	    if (len(split_responses) == 0):
		    return 0
		    
		#If the above condition is not met,
		#then there are some errors
		#Count the number of responses equal to 0
		data_response_0 = len(split_responses[split_responses == 0])
		#Count the number of responses equal to 1
		data_response_1 = len(split_responses[split_responses == 1])
		#Whichever class is less represented are the errors
		if (data_response_0 >= data_response_1):
			errors = data_response_1
		elif (data_response_1 > data_response_0):
			errors = data_response_0
		
		return errors
	    

## Determining the best split
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

## Creating the leaf nodes
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

## Build the tree
We will now introduce another stopping criteria called the max depth, which will prevent the tree from creating too many intermediate nodes and prevent overfitting.

    def build_tree(data, features, response, current_depth = 0, max_depth = 10):

		#Determing which features haven't been used for splitting
		remaining_features = features[:]
		#Define the response variable
		response_values = data[response]

		#Stopping condition 1:
		#Check if there are errors in the node
		#If there are no errors, we stop splitting
		#and create a leaf
		if (split_errors(response_values) == 0):
			print("Stopping condition 1 reached.")
			return create_leaf(response_values)
			
		#Stopping condition 2:
		#Check if there are any features remaining to split on
		#If we already split every feature,
		#create a leaf
		if (len(remaining_features) == 0):
			print("Stopping condition 2 reached.")
			return create_leaf(response_values)
		
		#Stopping condition 3:
		#Check if the max depth has been reached
		#If we already reached the limit,
		#create a leaf
		if (current_depth >= max_depth):
			print("Stopping condition 3 reacheed.")
			return create_leaf(response_values)
		
		#If we made it this far, it means we can continue splitting
		#We can now determine the best split
		splitting_feature = best_split(data, features, response)
		
		#Define the left and right splits
		left_split = data[data[splitting_feature] == 0]
		right_split = data[data[splitting_feature] == 1]

		#Remove the feature we just split on from consideraion
		#for future splits
		remaining_features.remove(splitting_feature)

		#Check to see if the split is perfect
		#If so, create a leaf
		if (len(left_split) == len(data)):
			print("Perfect split.")
			return create_leaf(left_split[response])
		if (len(right_split) == len(data)):
			print("Perfect split.")
			return create_leaf(right_split[response])
		
		#This concludes one iteration
		#We need this function to be recursive
		#Always checking stopping conditions and
		#testing for the next best split
		#Thus, we call this same function within the function
		#We do it for the left branch and the right branch
		left_tree = build_tree(left_split, remaining_features, response, current_depth + 1, max_depth)
		right_tree = build_tree(right_split, remaining_features, response, current_depth + 1, max_depth)

		#Return a dictionary of items from this iteration
		return {'is_leaf': False,
				'prediction': None,
				'spliting_feature': splitting_feature,
				'left': left_tree,
				'right': right_tree}
		
## Classify Observations
Building the tree does not output any performance metrics. We need to use the tree's structure to classify observations as either 0 or 1. Once we do this, we can then evaluate the effectiveness of the tree.

    def classify_observations(tree, data, annotate = False)
	    
	    #If the tree split is a leaf, return the prediction
	    if tree['is_leaf'] == True:
		    return tree['prediction']
		
		#If the tree is not a leaf, continue searching for leaf
		else:
			split_value = data[tree['splitting_feature']]
			if (split_value == 0):
				return classify_observations(tree['left'], data, annotate)
		    else:
			    return classify_observations(tree['right'], data, annotate)
		

## Evaluate The Tree
(to be continued)
    


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgyMzg1NjAzNSwtNDUxODU1MTYwLC0xND
UxNTc0MzEzXX0=
-->