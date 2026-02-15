# `models`

## `SampleDistClustering`

```
Clustering based on subsampling and distance metrics.

-------------------
Constructor method
-------------------

Parameters: (inputs)
-----------

clustering_method: the base clustering algorithm instance (e.g., a scikit-learn or kmedoids object) to be fitted on the subsample's distance matrix.

metric: the global distance metric to be computed. Must be an string (e.g., 'minkowski', 'robust_mahalanobis', or a mixed metric name).

frac_sample_size: the sample size in proportional terms to be extracted for the initial clustering. Must be a float in (0, 1].

random_state: the random seed used for extracting the (random) sample elements.

stratify: whether to use stratified sampling based on the response variable `y`. Must be a boolean.

p1, p2, p3: number of quantitative, binary and multi-class variables in the considered data matrix, respectively. Must be non-negative integers.

d1: name of the distance to be computed for quantitative variables.

d2: name of the distance to be computed for binary variables.

d3: name of the distance to be computed for multi-class variables.

q: the parameter that defines the Minkowski distance. Must be a positive integer.

robust_method: the method to be used for computing the robust covariance matrix. Only needed when metric or d1 = 'robust_mahalanobis'.

alpha: a real number in [0,1] used by the robust covariance estimation method. Only needed when metric or d1 = 'robust_mahalanobis'.

-----------
Fit method: 
-----------

Fits the subsample-based clustering algorithm to `X`, and `y` (if stratification is required).

Parameters: (inputs)
-----------

X: a pandas/polars data-frame or a numpy array. Represents a predictors matrix and is required. 
If using mixed metrics, the first p1 predictors must be the quantitative, followed by the p2 binary predictors, and finally the p3 multiclass predictors.

y: a pandas/polars series or a numpy array. Represents a response variable. Only required if `stratify=True`.

weights: the sample weights. Used internally for robust covariance estimation if metric or d1 = 'robust_mahalanobis'. 

---------------
Predict method:
---------------

Predicts clusters for `X` observations by assigning them to their nearest medoid (found during the fit stage) according to the configured metric.

Parameters: (inputs)
-----------

X: a pandas/polars data-frame or a numpy array. Represents a predictors matrix and is required. 
Must follow the same column structure as the `X` passed to the fit method.

Returns: (outputs)
--------

predicted_labels: a list containing the predicted clusters of each observation of `X`.
```



## `FoldSampleDistClustering`

```
K-Fold Meta-Clustering based on subsampling and distance metrics.
Implements a two-stage ensemble algorithm:
1. Splits data into K folds and finds local medoids for each partition.
2. Pools all local medoids and clusters them again to find the global meta-medoids.
3. Reconstructs the final labels mapping back to the original points.

-------------------
Constructor method
-------------------

Parameters: (inputs)
-----------

clustering_method: the base clustering algorithm instance (e.g., a scikit-learn or kmedoids object) to be fitted on the distance matrices of each fold and the final meta-clustering stage.

metric: the global distance metric to be computed. Must be a string (e.g., 'minkowski', 'robust_mahalanobis', or a mixed metric name).

n_splits: number of folds to be used for partitioning the data. Must be an integer greater than 1.

shuffle: whether data is shuffled before applying KFold. Must be a boolean.

random_state: the random seed used for extracting sample elements and, if shuffle=True, for KFold partitioning.

stratify: whether to use stratified sampling based on the response variable `y` within each fold. Must be a boolean.

frac_sample_size: the sample size in proportional terms to be extracted within each fold for local clustering. Must be a float in (0, 1].

meta_frac_sample_size: the sample size in proportional terms to be used during the meta-clustering stage (pooling all local medoids). Must be a float in (0, 1].

p1, p2, p3: number of quantitative, binary and multi-class variables in the considered data matrix, respectively. Must be non-negative integers.

d1: name of the distance to be computed for quantitative variables.

d2: name of the distance to be computed for binary variables.

d3: name of the distance to be computed for multi-class variables.

q: the parameter that defines the Minkowski distance. Must be a positive integer.

robust_method: the method to be used for computing the robust covariance matrix. Only needed when metric or d1 = 'robust_mahalanobis'.

alpha: a real number in [0,1] used by the robust covariance estimation method. Only needed when metric or d1 = 'robust_mahalanobis'.

-----------
Fit method:
-----------

Fits the 2-stage meta-clustering model to `X` (and `y` if stratification is required).

Parameters: (inputs)
-----------

X: a pandas/polars data-frame or a numpy array. Represents a predictors matrix and is required. 
If using mixed metrics, the first p1 predictors must be the quantitative, followed by the p2 binary predictors, and finally the p3 multiclass predictors.

y: a pandas/polars series or a numpy array. Represents a response variable. Only required if `stratify=True`.

weights: the sample weights. Used internally for global robust covariance estimation if metric or d1 = 'robust_mahalanobis'. 

---------------
Predict method:
---------------

Predicts clusters for `X` observations by assigning them to their nearest global meta-medoid (found during the second stage of the fit process) according to the configured metric.

Parameters: (inputs)
-----------

X: a pandas/polars data-frame or a numpy array. Represents a predictors matrix and is required. 
Must follow the same column structure as the `X` passed to the fit method.

Returns: (outputs)
--------

predicted_labels: a list containing the predicted clusters of each observation of `X`.
```
