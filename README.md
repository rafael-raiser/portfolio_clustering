# Classification by clustering

# Abstract
In this project we analyze a stellar dataset containing observational information of stars, galaxies and quasars using principal component analysis and K-Means clustering. Our goal is to test if the clustering method is able to identify the categories of the objects based solely in the physical measurements.

 **clustering.ipynb**: code containing data analysis by pca and k-means clustering.

 # Clustering

Clustering is a procedure used to classify data into subsets containing similiar itens. There are different types of clustering algorithms (Figure 1), each one more or less efficient in classifying certain dataset according to its topology.

 ![figure1](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/clustering_algorithms.webp)

(Figure 1: Clustering Algorithms. Source: [Scikit-learn](https://scikit-learn.org/stable/modules/clustering.html))

In this project, we use K-Means clustering which is based on centroids identification (Figure 2). The main input of this method is the number of clusters (n_clusters) and it works by the following steps:

1. N random points are initialized as centroids;

2. Each point of the dataset gets associated with the closest centroid;

3. New centroids are defined as the center of mass of the clusters;

4. Steps 2 and 3 are repeated untill the centroids stop varying.

K-Means method has the avantage of its simplicity and the guarantee of converge. However, due to the own nature of the algorithm, it looses efficiency if the clusters are not spherically distributed.

![figure2](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/kmeans_clustering_example.gif)

(Figure 2: K-Means clustering algorithm. [Source](https://theanlim.rbind.io/post/clustering-k-means-k-means-and-gganimate/).)

As the clustering works with points representing data in a N-dimensional space, it's important that the dataset is subject to padronization. PCA usually is very usefull in this kind of analysis by its propertie of reducing the data dimensionality and concentrating the variance in few principal components.

# Clustering applied to the SDDS17 Stellar Dataset

We are now going to analyze the SDDS17 Stellar Classification Dataset (available in [kaggle](https://www.kaggle.com/datasets/fedesoriano/stellar-classification-dataset-sdss17/)), containing 100,000 entries classified in three categories: stars, galaxies and quasars (quasi-stellar objects). We use only the eight parameters related to physical measurements in this analysis: celestial coordinates alpha and delta, photometric measurements u, g, r, i, z and redshift.

Firstly, we apply PCA over the dataset in order to obtain enough principal components to explain 95% of the variance, reducing the 8 parameters into 5 PC's. Figure 3 (left) shows a 3D plot of PC's 1, 2 and 3. It's clear that an outlier is present in the dataset; it's removed and PCA is done again (Figure 3, right). The explained variance ratios of the first 5 components are then:

\[0.37797983, 0.28362062, 0.14261839, 0.10769035, 0.08348138\]

![figure3a](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/PCA_3D_outlier.png)
![figure3b](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/PCA_3D_cleaned.png)

(Figure 3: Plot of PC1, PC2 and PC3 before (left) and after (right) data cleaning)

In order to make the visualization better, we also perform 2D plots relating PC's 1, 2 and 3 with colors representing the 3 classes of objects (Figure 4),

![figure4](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/PCA_2D_classes.png)

(Figure 4: 2D plots of PC1, PC2 and PC3 for different classes of stellar objects)

We then apply K-Means clustering over the dataset assuming n_clusters=3 and each point is associated with a cluster. The distribution of the clusters representing each class of stellar object is shown in Figure 5.

![figure5](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/pie_charts_kmeans.png)

(Figure 5: Distribution of the objects classes into each cluster)

Although the algorithm was able to differentiate quasars (identified with label 1) from the other objects, galaxies and stars were poorly resolved and both are distributed among clusters 0 and 2. Physically speaking this distinction makes sense: quasars, or quasi-stellar objects, are extremely luminous and active galactic nuclei (AGN) powered by accretion onto supermassive black holes; they are much more frequent in the beginning of the universe and are observed with high redshift, at larger distances and with shifted spectra when compared to stars and galaxies.

Figure 6 shows the same plot as Figure 4 but with points separated by colors according to the K-Means results.

![figure6](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/PCA_2D_clusters.png)

(Figure 6: 2D plots of PC1, PC2 and PC3 for the 3 clusters)

Finally, we can evaluate the optimal number of clusters. To do so, we calculate the so called silhouette coefficient (see [Wikipedia](https://en.wikipedia.org/wiki/Silhouette_(clustering)#:~:text=The%20silhouette%20value%20is%20a,poorly%20matched%20to%20neighboring%20clusters.)), a scale varying from -1 to +1 representing poor and perfect clustering, respectively.

As the clustering quality is proportional to the coefficient, we can estipulate the ideal number of clusters by observing the point where the curve gets an elbow shape. In other words, we look for a value of n_clusters for which n_clusters+1 doesn't vary too much. In the case of our dataset, it seems that the optimal number is 4 (Figure 7).

![figure7](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/scores_kmeans.png)

(Figure 7: Silhouette score for n_cluster ranging from 2 to 7)

# Conclusions

By applying principal component analysis and K-Means clustering on the dataset, we were able both to clean the data from outliers and to categorize it in a way that makes physical sense: quasars were put inside a single cluster while stars and galaxies, both with very different characteristics from the first ones, were poorly resolved within 2 clusters due to their similarity.








 

 
