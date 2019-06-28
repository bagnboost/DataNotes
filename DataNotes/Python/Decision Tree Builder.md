## Decision Tree Builder

### Categorical Features

#### Define categorical features

	categorical_features = ['x1', 'x2','x3','x4',...]
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
	

    


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYyODk0OTM4Nl19
-->