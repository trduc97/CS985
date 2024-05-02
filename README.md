This notebook was created to compete in Kaggle's [Spotify Classification Problem 2024](https://www.kaggle.com/competitions/cs9856-spotify-classification-problem-2024)

The competition is strictly focused on performing training using the provided 452 songs, with 12 features, to classify the songs into 88 genres. Only feature engineering using the given 12 features is allowed, no form of data enrichment from external sources is permitted. For enrichment solution of the given data, please explore the notebook [spotify_enrich]([url](https://github.com/trduc97/Spotify_classification/blob/main/spotify_enrich.ipynb)) which uses the Spotipy library to crawl additional information related to the songs. 

2. EDA 
The given data suffered from the following issues and our solution:

| Issue                           | Solution                                                                                   |
|---------------------------------|-------------------------------------------------------------------------------------------|
| Duplicated values               | Remove duplicated values                                                                  |
| Imbalanced classes              | Rebalanced the weights of each class in the models<br>(Considered other methods such as over/under/synthetic sampling but with many classes at 1 sample, weight balancing is the optimal solution) |
| Outliers and skewed distribution | Applied Box-cox transformation on 'spch', 'live', 'dur' columns,<br>and Yeo-Johnson transformation on "acous" column |
| Missing values on the labels column | Manually filled out as these are the target features,<br>applying any prediction methods on these would just mean switching these values to the test set |

3. Features engineering

| Feature | Feature engineering
|---------------------------------|-------------------------------------------------------------------------------------------|
| year | Applied binning every 5 years into 1 period |
| title | Applied TF-IDF to look for some keywords that specify certain genres + SVD to reduce size|
| artist | Applied one-hot encoding for the whole name since some artists will be specialized in certain genres + SVD to reduce size|

4. Models and results
we applied hyperparameter tuning on various models from different family and got accuracy as below for the 88 classes classification problem: 

![single_model_results](https://github.com/trduc97/Spotify_classification/assets/52210863/496e199d-c1bf-43c6-b9c0-7f2ab81670ca)

From the tuned models, we perform ensembled classifying using hard-voting and soft-voting to achieved the result:

Ensembled model results:
- Hard-voting classifier accuracy:  0.5824
- Soft-voting classifier accuracy:  0.6373

5. [Competition results]([url](https://www.kaggle.com/competitions/cs9856-spotify-classification-problem-2024/leaderboard))
- Hard-voting classifier accuracy: 57.14% <= co-2nd place results 
- Soft-voting classifier accuracy: 53.57%

Additional note: 
1. On grouping the labels to enhanced classification results
With 88 groups, in reality we can considered grouping the genre

  
3. On applying Spotipy to enrich dataset
