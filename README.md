# Module-Challenge-10: Crypto Clustering 

## Import the Data
### 1. For this module, we import pandas, hvplot, Path, KMeans from sklearn.cluster, PCA from sklearn.decomposition, and StandardScalar from sklearn.preprocessing.

### 2. We next import the data from crypto_market_data.csv to a pandas dataframe using pd.read_csv and setting the index column to "coin_id," followed by using the .head(10) function. For summary statisitics, the .describe() function was applied to the created dataframe, in this case df_market_data.

### 3. To plot the dataframe, we use hvplot with the .line() functions and parameters of height, width, and rot. 

## Prepare the Data
### 1. To normalize the data, we use the .fit_transform() function with standard scalar as follows:

scaled_data = StandardScaler().fit_transform(df_market_data)

### 2. With the newly-scaled data, we create a dataframe, df_market_data_scaled, and copy the index of "coin_id" from df_market_data with df_market_data.index() assigned to df-market_data_scaled["coin_id"]. Next, we officially set the df_market_data_scaled index with df_market_data-scaled.det_index("coin_id"), then previewed with the .head() function.

## Find the Best Value for k Using the Original Data
### 1. First, we create a list of k values to try with k = list(range(1,11)), followed by an empty list for inertia = [].

### 2. To compute each possible inertia, we create a for loop with the following parameters

for i in k:

    model = KMeans(n_clusters=i, random_state=1)
    
    model.fit(df_market_data_scaled)
    
    inertia.append(model.inertia_)

### Where were have an internal variable for model assigned to Kmeans with n_clusters=i and random_state=1. The model is fitted in the loop, with the inertia appended in the model

### 3. For elbow_data, we create a list with "k" for k and "inertia" for inertia. Using pd.Dataframe, elbow_data is made into a dataframe and represented by df_elbow variable.

### 4. To plot the elbow curve and determine the best k-value, we get the following:

df_elbow.hvplot.line(x="k", y="inertia", title="Elbow Curve", xticks=k)

### Where our axes are defined by "k"and "inertia." For response answer, please refer to the notebook.

## Cluster Cryptocurrencies with K-means Using the Original Data
### 1. From the written answer, we got a k of 4. We now create a model variable assigned to KMeans with parameters of n_clusters=4 and random_state=1. To fit the model, we next use model.fit(df_elbow).

### 2. To predict the crypto clusters, we now make a scaled_crypto variable assigned to model.predict(df_elbow), followed by a print statement. For a copy of the dataframe, we use df_elbow.copy(). We need a new column for cluster predictions in scaled_crypto_copy, which is done through the following code:

scaled_crypto_copy['Cluster Predictions'] = scaled_crypto

scaled_crypto_copy.tail()

### 3. To graph a scatter plot of the data, we enter:

cluster = df_market_data_scaled.hvplot.scatter(x="price_change_percentage_24h", y="price_change_percentage_7d", hover_cols="coin_id", by ="coin_id")

cluster

### where we are told which axes to use and using hover_cols and by assigned to "coin_id" for proper labeling and colors.

## Optimize Clusters with Principal Component Analysis
### 1. We first make the PCA model with PCA(n_components=3). Similar to earlier, we now used pca.fit_transfrom(df_market_data_scaled) assigned to market_paca_data followed by market_pca_data[:5] to preview. For the variance ratios, we simply use pca.explained_variance_ratio_ (refer to notebook for written answers). 

### 2. We now need a market pca dataframe, which can be seen with:

market_pca = pd.DataFrame(

    market_pca_data,
    
    columns=["PC1", "PC2", "PC3"],
    
    index=df_market_data_scaled.index) 

### Note, from the written answer in the notebook and the variance ratios, we have three principle components, each needing their own column. To simply copy "coin_id" as the index in our new dataframe, we use index = df_market_data_scaled.index, followed by reviewing the data with .head()

## Find the Best Value for k Using PCA Data
### 1. The first steps are almost identical from before, with setting up k, inertia, and the for loop:

k = list(range(1, 11))

inertia = []

for i in k:

    model = KMeans(n_clusters=i, random_state=1)
    
    model.fit(market_pca)
    
    inertia.append(model.inertia_)

### Also like before, we create an elbow data dictionary and dataframe:

pca_elbow_data = {
    "k": k,
    
    "inertia": inertia
    
}

pca_df_elbow = pd.DataFrame(pca_elbow_data) 

### 2. To plot this elbow curve, like before, we use hvplot.line with "k" and "inertia" as axes parameters, a title, and xticks.

## Cluster Cryptocurrencies with K-means Using the PCA Data 
### 1. We found the best k-value to be 4 again, and like before we set up our model steps:

model = KMeans(n_clusters=4, random_state=1) 

model.fit(market_pca)

### Also like before, we need to predict the clusters through the following code:

k = model.predict(market_pca)

print(k)

### Like before, we need a copy of the PCA Data, create a "predicted Clusters" column, and review the result as shown below:

market_pca_predictions = market_pca.copy()

market_pca_predictions["Predicted Clusters"] = k

market_pca_predictions.head()

### 2. In order to make a scatter plot, we will need, based on the prompt, x, y, by, and hover_cols parameters along with hvplot. This can be done as shown below:

cluster_pca= market_pca_predictions.hvplot.scatter(

    x = "PC1",
    
    y = "PC2",
    
    by = "Predicted_Clusters",
    
    hover_cols=['coin_id']
)

cluster_pca

## Visualize and Compare the Results
### 1. Simply for both, we use use a similar process with the + operator:

composite_plot = inertia_plot + inertia_pca

composite_plot

composite_plot_two = cluster + cluster_pca

composite_plot_two

For written answer, please refer to the notebook.




