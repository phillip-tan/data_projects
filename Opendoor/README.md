# k-NN Strikes Back!
1. The goal of this project is to build a k-NN model using latitude and longitude data to predicting the closing prices of homes.
2. The housing data can be found under **data.csv**  and a more detailed description of the project is titled **kNN_housing.pdf**
3. **kNN_Strikes_Back.ipynb** is the jupyter notebook of the working code, as well as the entire workflow of the project.

## Future Considerations
### An appropriate methodology to determine optimal k
1. One approach is to run this test set on a range of different values of k and graph the model performance based on MRAE to see which value of k is optimal. 
2. There could potentially be an approach that invovles cross-validation, but because each testing datapoint uses a different training set (the newer the home, the more training data), it wouldn't be as reliable. 
3. The first and simpler approach would work best, even in production.
### Spatial or Temporal Trends in Error
1. The raw data, not including the negative closing prices, has two ostensible clusters: homes both over and under a million dollars, with homes under a million being the overwhelming majority. Not specifying the price range in the data worsens the error. It's possible that the homes over a million dollars (some are ten and other a hundred million) represent a spatial trend, where the multi-million dollar homes are of a different location than the rest. 
2. To find out temporal trends, one would plot the datetime by its respective relative absolute error. To visualize, one could group the datetime by months or seasons to see how temporal trends affects the variance in closing prices.
3. To visualize spatial trends in error, one would plot the geolocation data on a map graph and superimpose it with a color scale of respective errors.
### Model Improvement
1. K-D Tree. A k-d tree would be able to spatially store the training data for faster look-up times of nearest neighbors. In turn, this will decrease testing time. Especially given that the dimensionality of this problem is low, a k-d tree is ideal. 
2. Increase features and more data. As of now, the model is simply predicting based on geolocation data. For example, it's possible that the model is predicting the price of a shack next to four mansions. To improve it, there must be more information about the homes (i.e., square feet, bedrooms, school district ratings). It's worth noting that once dimensionality increases past about twenty, locality sensitive hashing (LHS) will temper the "curse of dimensionality."
3. If possible to create an efficient algorithm, one could try to improve the distance accuracy by using the great circle equation instead of the reduced haversine equation.
4. One could explore alternatives to Shepherd's method of the inverse distance weighting scheme. For example, this model uses p=2 but it could be decided to put either a higher or lower penalty on neighbors that are further away. Other metrics for the IDW scheme would be the Łukaszyk-Karmowski metric or Modified Shepherd's method. The Modified Shepherd's method is ideal to combine with a k-d tree because this method searches for nearest neighbors within a certain sphere of the entire sample, and when coupled with a k-d tree the complexity becomes N*logN time. 
### Productionizing this model
1. Everything listed under "Model Improvement."
2. The productionized model should incorporate into its algorithm an additional weight for a particular home on its neighbors that were sold more recently. 
3. Assuming that in production there will be a lot more features, it's necessary to rescale the data for features that are in different metrics. For starters, scikit-learn has a normalize() method.
4. Finally, this model should be able to handle live data. It could use an API that was written to query a database accordingly for new houses that are being added.
