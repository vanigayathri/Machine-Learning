# KMeans Clustering with Jaccard Index

## Objective
Our objective was to apply K-means clustering to datasets containing documents, exploring different values of K to determine the optimal number of clusters. We employed the Jaccard index, a similarity metric ranging from 0 to 1, which assesses similarity based on the overlap of words present in two documents. This index is particularly suitable for sparse binary datasets, as it considers common or disjoint elements in two sets.

## Metric
We utilized the Jaccard distance as a dissimilarity metric between documents, replacing the Euclidean distance in clustering tasks to identify clusters within given documents.

## Procedures
We primarily employed two procedures for clustering:

1. **Using the Set of Words in a Document**
2. **Using the Bag of Words with Frequencies**

In both procedures:
- Data was stored in a dictionary, where each document ID served as a key.
- For the first procedure, the value was the set of words, and for the second procedure, it was a dictionary with the word ID as the key and the frequency as the value.
- We randomly selected 'k' centroids.
- Clusters were created based on the Jaccard distance between the points and the centroids.
- New centroids for the clusters were created either for a fixed number of iterations or until the difference between the new centroids and old centroids was below a certain threshold.

### Updating Centroid in Procedure 1:
For each word in the cluster, we calculated the number of documents it appeared in. Then, we selected the top 'N' words, sorted by their frequency of occurrence across documents, where:

N = (Sum of No. of unique words in each document in the cluster) / (Total no. of documents in the cluster)

### Updating Centroid in Procedure 2:
We tried multiple approaches:

**Approach 1:**  
- Calculated the occurrence of each word in the cluster.
- Computed 'count_n', representing the average number of words in each document within the cluster.
- Sorted words in descending order of frequency.
- Selected the first 'count_n' elements from this list.
- Encountered issue: Clusters converged to only one cluster after several iterations.

**Approach 2:**  
- Calculated the occurrence of each word in the cluster.
- Computed 'count_n', representing the average number of words in each document within the cluster.
- Selected the top 'count_n' words, sorted by their frequency.
- Adjusted their frequency by dividing by the number of documents in the cluster.
- Encountered issue: Inertia varied significantly even with an increase in the number of clusters.

**Final Approach:**  
- Calculated the occurrence of each word in the cluster.
- Computed 'count_n', representing the average number of words in each document within the cluster.
- Selected the top 'count_n' words, sorted by their frequency.
- Adjusted their frequency by dividing by the number of documents in the cluster.
- The resulting words with adjusted frequency formed the updated centroid.

## Results
We plotted the graph of inertia vs K for different thresholds (0.1, 0.2, 0.05, 0.3, etc.) and different random states (69, 56, 100, 67, 14, etc.). Of all these combinations, the best Inertia vs K graphs have been shown below for each dataset.

For Procedure 1, we identified two values of K for the KOS dataset, 4 and 11, where there was a decrease in the rate of change of inertia. However, we opted for 11 over 4 because a greater number of clusters typically leads to more homogeneity within the clusters.

| Collection | K   | Time Taken (secs) | Space Taken (KB)  | Threshold | Seed |
|------------|-----|-------------------|-------------------|-----------|------|
| Enron      | 5   | 345.31            | 371428            | 0.1       | 69   |
| Nips       | 4   | 113.65            | 5148              | 0.1       | 56   |
| Kos        | 11  | 50.72             | 10384             | 0.2       | 100  |

![image](https://github.com/ani98622/KMeans_Jaccard_Index_from_scratch/assets/141315211/13ef53a1-af39-46b5-94ae-5aa8fe0d9036)
![image](https://github.com/ani98622/KMeans_Jaccard_Index_from_scratch/assets/141315211/1045a8e1-7b16-415e-8e6a-aada328d161a)

For Procedure 2 , results are as follows: 

| Collection | K   | Time Taken (secs) | Space Taken (KB)  | Threshold | Seed |
|------------|-----|-------------------|-------------------|-----------|------|
| Enron      | 5   | 45.8              | 220912            | 0.2       | 67   |
| Nips       | 10  | 94.6              | 15708             | 0.2       | 14   |
| Kos        | 11  | 137.52            | 40788             | 0.05      | 100  |

![image](https://github.com/ani98622/KMeans_Jaccard_Index_from_scratch/assets/141315211/df290432-aa21-4a14-8147-c2acaac506e8)


