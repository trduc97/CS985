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

5. [Competition results](https://www.kaggle.com/competitions/cs9856-spotify-classification-problem-2024/leaderboard)
- Hard-voting classifier accuracy: 57.14% <= co-2nd place results 
- Soft-voting classifier accuracy: 53.57%

Additional note: 
1. On grouping the labels to enhance classification results

On close inspection of the 88 genres, we see multiple sub-genres of rock, pop, classical, .... many of which only have 1,2 songs. Intuitively, you can't realistically understand a genre after only ever listening to 1 song, so we considered grouping the less represented genres into the more populated genres, like all rock music into a single classic rock, to improve the classification result of the more populated classes, in hope to enhance the overall classification results. We tried this approach in the 6th section of the notebook [label_grouping](https://github.com/trduc97/Spotify_classification/blob/main/label_grouping.ipynb).
First, we identify the top genres, then based on the cosine similarity group the less represented genres to the top genres with a certain level of similarity.
Finally, we use a neural network for an exhaustive classification solution of the genres to test for feasibility. 

We tried different combinations: 
- Group bottom genres into the top 10/top15; only group the bottom genres into the top 4th to 10th (since the top 3 already has plenty)
- Different levels of similarity of 70%/80%/90%

The results are overall all  worse with the genres mixed, indicating this will only reduce the overall available information.

  
2. On applying Spotipy to enrich the dataset

When using Spotify's API to crawl the data, we can receive very crucial information to aid our task of genre classification, which is the artists' genre. Intuitively, artists will specialised in a small number of genres, which helps narrow down the process significantly (In the end, the use of this information is strictly prohibited within the competition so the results above do not incorporate this info)

Originally, the data was taken from this [dataset](https://www.kaggle.com/datasets/cnic92/spotify-past-decades-songs-50s10s), which was curated from the series of playlists [All Out 50s](https://open.spotify.com/playlist/37i9dQZF1DWSV3Tk4GO2fq),[All Out 60s](https://open.spotify.com/playlist/37i9dQZF1DXaKIA8E7WcJj),[All Out 70s](https://open.spotify.com/playlist/37i9dQZF1DWTJ7xPn4vNaz),[All Out 80s](https://open.spotify.com/playlist/37i9dQZF1DX4UtSsGT1Sbe),[All Out 90s](https://open.spotify.com/playlist/37i9dQZF1DXbTxeAdrVG2l),[All Out 2000s](https://open.spotify.com/playlist/37i9dQZF1DX4o1oenSJRJd),[All Out 2010s](https://open.spotify.com/playlist/37i9dQZF1DX5Ejj0EkURtP)

But the Kaggle dataset does not include the songs' ID key on Spotify, and from the time of the Kaggle dataset's creation on 28/01/2020, the 7 playlists have drastically changed due to changing trend, and until the time of this report, only ~30% of the songs in the dataset remained on the Spotify playlists. This poses a serious issue when crawling the additional data from Spotify's API, since without the original songs' keys, we can easily receive wrong results, such as a remix/remasted/cover. We consider patching the wrong results by creating our playlists for the missing data in the [training](https://open.spotify.com/playlist/1dbT3PmOUwKDeAcPzC0Xne) and [testing](https://open.spotify.com/playlist/5WKqU4AK7KjYWYrKOmRGCv) datasets. 
