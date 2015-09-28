This project includes all the code used to analyze the data of the [Yelp Dataset Challenge](http://www.yelp.com/dataset_challenge) during the Data Mining Capstone Project of the Data Mining Specialization in Coursera.

The project is organized in folders as folows.

- data. This folder should contain the Yelp dataset. But due to the data license agreement, it cannot be distributed.
- scripts. R scripts used to analyze the data.
- results. This folder will hold the intermediate results generated by the scripts in the script folder.
- reports. Project milestone reports.
- tools. Downloaded command line tools.

# Instructions

To reproduce the analysis, first unzip the Yelp JSON files in the data folder and then run the following scripts for each task.

## Task 1. Exploratory analysis.

1. **scripts/exploratory/read.data.R** Reads Yelp data in JSON format and saves the resulting data.frames in RData files.

2. **scripts/exploratory/restaurant_id.R** Filters business by category to keep only those labeled as Restaurants and then filters the reviews to keep reviews for restaurants. Stored the ids of the filtered businesses and reviews.

3. **scripts/exploratory/review.model.language.R** Computes a unigram language model for a corpus of Yelp reviews.

4. **scripts/exploratory/restaurants_model_language.R** Subsets the unigram model language to keep only a model language for a corpus of restaurant reviews. The vocabulary is also reduced to the most frequent words that account for 99% of the corpus.

5. **scripts/exploratory/topicmodel.R** Computes a topic model for the corpus of restaurant reviews.

6. **scripts/exploratory/data_by_rating.R** Splits the corpus of restaurant reviews into two corpora. One for positive reviews (stars >3) and another for negative reviews (stars < 3).

7. **scripts/exploratory/topicmodel_positive_negative.R** Computes a topic model for the positive and for the negative restaurant reviews corpora.

8. **scripts/exploratory/subset_useful_restaurant_reviews.R** Selects restaurant reviews that are useful. One review is useful if has been upvoted as usefull at least once.

9. **reports/01-Exploratory.Rmd** knits the exploratory analysis report 

## Task 2. Cuisine Clustering and Map Construction

1.  **scripts/cuisine_clustering/data_by_cuisine.R** Splits the reviews in restaurant by restaurant category. It removes those categories that are not clearly related to restaurants like Taxi and Automotive.

2.  **scripts/cuisine_clustering/cuisines_subsample.R** Loads the document-term matrix of restaurant reviews and saves a subset dtm of positive useful reviews for a subset of restaurant categories.

3. **scripts/cuisine_clustering/dtm_by_cuisine_aggregated.R** Aggregates the DTM of positive useful restaurant reviews by cuisine label. For that it concatenates all the documents for each label.

4. **scripts/cuisine_clustering/cuisine_aggregated_topic_model.R** Computes topic model with k=20 for dtm of concatenated positive useful restaurant reviews of a subset of restaurant categories.

5. **scripts/cuisine_clustering/similarities_aggregated.R** Computes similarities between cuisine categories using concatenated positive useful restaurant reviews. It tries three approaches: 1. cosine similarity of raw Term Frequencies. 2. BM25+ 3. Cosine similarities of probability distribution of each cuisine over the topics of a topic model fitted to the corpus using LDA.

6. **scripts/cuisine_clustering/topicmodel_by_cuisine.R** # Computes topic model with k=100 for dtm of positive useful restaurant reviews of a subset of restaurant categories.

7. **scripts/cuisine_clustering/similarities_not_aggregated.R** Computes similarities between cuisine categories using positive useful restaurant reviews. It tries three approaches: 1) Cosine similarities of probability distribution of each cuisine over the topics of a topic model fitted to the corpus using LDA. 2) cosine similarity of raw Term Frequencies. 3) BM25+ This script is computationally intensive and requires to be run on a computer with GPU computing capabilities.

8. **scripts/cuisine_clustering/cuisine_networks.R** Extracts backbone graphs from similarity matrices to create cuisine maps and applies community detection algorithms to cluster the cuisines.

9. **reports/02-Cuisine_Map.Rmd** knits a report about cuisine maps.

## Task 3. Dish Discovery

1. Enter on each of the cuisine folders in **scripts/dish_recognition/** and run the corresponding **cuisine_subset.R** script to create a corpus for each cuisine.

2. Copy from each of the cuisines folders in **results/dish_discovery/** the **cuisine_positive_useful_corpus.txt** and paste in the data corresponding input folder of TopMine and SegPhrase.

3. Run TopMine and SeghPhrase on each of the corpora. Only change the input files and keep all the remaining parameters as they are. The labeled samples for SegPhrase are located in **results/manualAnnotationTask/**

4. Enter on each of the cuisine folders in **scripts/dish_recognition/** and run the corresponding **cuisine_word2vect.R** script to run the Word2Vect algorithm on each of the corpora.

5. Run **scripts/dish_recognition/cross_cuisines.R** to combine all the algorithms results for each cuisine.

6. Run **reports/03-Dish_Discovery.Rmd** to knit a report about the results.
