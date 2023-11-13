# Classification by clustering
An example of classification by clustering applied to a stellar dataset.

# Abstract
In this project we analyze a stellar dataset containing a list of physical measurements of objects classified as stars, galaxies or quasars using Principal Component Analysis and clustering. Our goal is to test if the clustering method is able to identify the categories of the objects based solely in the measurements.

 **clustering.ipynb**: code containing data analysis by pca and k-means clustering.

 # Clustering

Clustering is a procedure used to classify data into subsets containing similiar itens. Existem diferentes tipos de algoritmos de clustering (Figure 1), cada um mais ou menos eficaz para classificar determinado conjunto de dados de acordo com sua topografia.

 ![figure1](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/clustering_algorithms.webp)

(Figure 1: Clustering Algorithms. Source: [Scikit-learn](https://scikit-learn.org/stable/modules/clustering.html))

Neste projeto, utilizaremos o algoritmo **K-Means** (Figure 2), baseado na classificação por centroides. Este tipo de método tem como input principal o número de clusters a serem classificados (N) e funciona de acordo com a seguinte lógica:

1. N pontos aleatórios são inicializados como centroides;

2. Cada ponto do conjunto é associado ao centroide mais proximo;

3. Novos centroides são definidos de acordo com o centro de massa dos conjuntos;

4. Os passos 2 e 3 são repetidos até a tolerância desejada.

O método de K-Means tem como vantagem sua simplicidade e garantia de convergência. Por outro lado, pela própria natureza do algoritmo, ele perde eficiência se os clusters não tiverem formato esférico.

![figure2](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/kmeans_clustering_example.gif)

(Figure 2: K-Means clustering algorithm. [Source](https://theanlim.rbind.io/post/clustering-k-means-k-means-and-gganimate/).)

Como os métodos de clustering de clustering operam sobre dados representados por pontos em um espaço N-dimensional, é importante que o conjunto seja submetido a um pre-processamento baseado na padronização dos dados. A análise de componentes principais geralmente é útil neste tipo de análise por reduzir a dimensionalidade dos dados e concentrar a variância do conjunto em poucas componentes principais.

# Clustering applied to a stellar dataset

Vamos agora analisar SDDS17 Stellar Classification Dataset, que apresenta medidas físicas (redshift, photometric data, celestial coordinates etc.) de objetos classificados em estrelas, galáxias e quasares.

Cada entrada do conjunto apresenta uma série de quantidades, algumas usadas como identificadores (marcadas com '\*' na lista abaixo) que serão descartadas em nossa análise:

*obj_ID = Object Identifier, the unique value that identifies the object in the image catalog used by the CAS 

alpha = Right Ascension angle (at J2000 epoch)

delta = Declination angle (at J2000 epoch)

u = Ultraviolet filter in the photometric system

g = Green filter in the photometric system

r = Red filter in the photometric system

i = Near Infrared filter in the photometric system

z = Infrared filter in the photometric system

*run_ID = Run Number used to identify the specific scan

*rereun_ID = Rerun Number to specify how the image was processed

*cam_col = Camera column to identify the scanline within the run

*field_ID = Field number to identify each field

*spec_obj_ID = Unique ID used for optical spectroscopic objects (this means that 2 different observations with the same spec_obj_ID must share the output class)

*class = object class (galaxy, star or quasar object)

redshift = redshift value based on the increase in wavelength

*plate = plate ID, identifies each plate in SDSS

*MJD = Modified Julian Date, used to indicate when a given piece of SDSS data was taken

*fiber_ID = fiber ID that identifies the fiber that pointed the light at the focal plane in each observation

Em primeiro lugar, aplicamos a análise de componentes principais com variância explicada maior que 95%, o que reduziu as 8 medições físicas a 5 componentes principais. Um plot 3D das 3 primeiras PC's (Figure 3) mostra claramente a presença de um outlier, que foi identificado e removido. A análise foi então feita novamente, e as taxas de variância relativas das 5 PC's são

\[0.37797983, 0.28362062, 0.14261839, 0.10769035, 0.08348138\]

![figure3a](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/PCA_3D_outlier.png)
![figure3b](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/PCA_3D_cleaned.png)

(Figure 3: Plot of PC1, PC2 and PC3 before (left) and after (right) data cleaning)

Para facilitar a visualização, plots 2D das PC1, PC2 e PC3 foram feitos com a identificação das 3 classes de objetos (Figure 4).

![figure4](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/PCA_2D_classes.png)

(Figure 4: 2D plots of PC1, PC2 and PC3 for different classes of stellar objects)

Por fim, aplicamos o método K-Means sobre o conjunto assumindo n_clusters=3. As frações de cada categoria de objeto nos clusters com label 0, 1 e 2 estão mostradas na Figura 6.

![figure5](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/pie_charts_kmeans.png)

(Figure 5: Distribution of the objects classes into each cluster)

![figure6](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/PCA_2D_clusters.png)

Embora o algoritmo tenha sido capaz de resolver quasares (identificados no subset com label 1) dos demais objetos, a diferença entre estrelas e galáxias é pouco clara e estes objetos estão distribuídos nos subsets 0 e 2. Fisicamente falando, esta distinção faz sentido: enquanto estrelas e galáxias são mais ou menos frequentes em todas as idades do universo, os quasares eram muito mais abundantes em seu início e, portanto, são observados com redshift muito maior (ou seja, a maioria se encontra muito mais distante).

A similaridade das observações para estrelas e galáxias fica mais evidente ao compararmos as Figuras 4 e 6, que mostram as componentes principais dividas entre classes de objetos e clusters. Devido à proximidade dos pontos representando estrelas e galáxias, o algoritmo K-Means não foi capaz de resolver corretamente as duas categorias.

(Figure 6: 2D plots of PC1, PC2 and PC3 for the 3 clusters)

Por fim, podemos avaliar o número ideal de clusters calculando o coeficiente de silhouette para n_clusters=2,3,4... (Figure 7). O coeficiente de silhouette (calculation details: [Wikipedia](https://en.wikipedia.org/wiki/Silhouette_(clustering)#:~:text=The%20silhouette%20value%20is%20a,poorly%20matched%20to%20neighboring%20clusters.)) é um número que varia de -1 até 1, com -1 indicando péssimo clustering e +1 clustering perfeito.

Como a qualidade do clustering é proporcional ao coeficiente, podemos determinal a quantidade ideal de clusters observando o ponto onde a curva tem elbow shape, ou seja, n_clusters tal que o score para n_clusters+1 diminui drasticamente ou se mantém aproximadamente constante. Em nosso caso, este número é n_clusters=4.

![figure7](https://github.com/rafael-raiser/portfolio_clustering/blob/main/images/scores_kmeans.png)

(Figure 7: Silhouette score for n_cluster ranging from 2 to 7)









 

 
