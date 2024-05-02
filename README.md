This notebook was created to compete in Kaggle's [Spotify Classification Problem 2024](https://www.kaggle.com/competitions/cs9856-spotify-classification-problem-2024)

The competition is strictly focused on performing training using the provided 452 songs, with 12 features, to classify the songs into 88 genres. Only feature engineering using the given 12 features is allowed, no form of data enrichment from external sources is permitted. For enrichment solution of the given data, please explore the notebook [spotify_enrich]([url](https://github.com/trduc97/Spotify_classification/blob/main/spotify_enrich.ipynb)) which uses the Spotipy library to crawl additional information related to the songs. 

2. EDA 
The given data suffered from the following issues and our solution:

| Issue                           | Solution                                                                                   |
|---------------------------------|-------------------------------------------------------------------------------------------|
| Duplicated values              | Remove duplicated values                                                                  |
| Imbalanced classes             | Rebalanced the weights of each class in the models                                       |
|                                 | (Considered other methods such as over/under/synthetic sampling but with many classes at |
|                                 | 1 sample, weight balancing is the optimal solution)                                       |
| Outliers and skewed distribution | Applied Box-cox transformation on 'spch', 'live', 'dur' columns,                         |
|                                 | and Yeo-Johnson transformation on "acous" column                                         |
| Missing values on the labels column | Manually filled out as these are the target features,                                    |
|                                 | applying any prediction methods on these would just mean switching these values to the   |
|                                 | test set                                                                                   |



3. Model
4. 


Additional note: 
