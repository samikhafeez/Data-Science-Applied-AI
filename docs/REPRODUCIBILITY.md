# Reproduction and evidence notes

The notebook, PDF report and presentation recording are retained without changes to their contents. The saved outputs describe historical execution, and the original environment was not supplied as a lockfile.

## Separate experimental paths

1. **UCI exploration:** cell 1 fetches dataset 17. Labels are mapped as malignant = 1 and benign = 0. IQR filtering uses the full exploratory data before a later split, leaving 486 observations. Cell 43 records 388 training and 98 test samples.
2. **Scikit-learn comparison:** cell 55 reloads the dataset through `load_breast_cancer`, sets up its own split and fits `StandardScaler` on training features. This dataset interface uses malignant = 0 and benign = 1. It has a different sample count from the filtered path.
3. **Search and evaluation cells:** cells 56–60 reuse names such as `X`, `X_train` and `y`. Some reload the dataset and others depend on earlier state. Their saved execution counters are nonsequential. Do not assume every printed score belongs to the same dataset preparation or execution session.

## Observed issues retained in the source

- **Saved error:** cell 50 has an empty current source but retains a `ValueError` output reporting inconsistent sample counts `[30, 388]`. This demonstrates a mismatch between saved output and current cell contents.
- **Preprocessing order:** outlier filtering and a normalisation expression occur before the split in the exploratory path. Data-dependent preprocessing should be fitted within training splits when establishing a new evaluation.
- **Overwritten features:** the PCA and t-SNE sections assign raw numeric features back to `X` after the earlier normalisation expression. A narrative about normalisation alone does not establish which representation a later experiment used.
- **Manual objective:** the manual logistic-regression loss expression differs from standard binary cross-entropy, and its saved run contains negative costs and an exponential-overflow warning. Preserve this as a learning experiment, not as a validated optimisation implementation.
- **Cross-validation preprocessing:** the SVM and KNN search cells scale all training rows before cross-validation, rather than putting scaling inside each fold with a pipeline. This is distinct from fitting the scaler on the final held-out test set, but still matters for interpreting cross-validation scores.
- **Metric meaning:** precision, recall and ROC-AUC in the final section refer to class 1, which is benign in that path. They should not be relabelled as malignant-case sensitivity.
- **Search cost:** the saved Random Forest search reports 648 candidates and 3,240 fits. Running all searches may take substantial time.

These observations are based on reading the source and stored outputs. They have not been resolved by changing the original notebook.

## Dependencies

Use Python 3.11 when investigating reproduction; the notebook records 3.11.7. `requirements.txt` is a proposed compatibility set assembled from actual imports. It is not the original environment and has not been installed or runtime-tested during repository preparation.

The scikit-learn version is restricted to 1.4.2 because that documented API accepts the notebook's `TSNE(n_iter=...)` argument. [Official scikit-learn 1.4.2 reference](https://scikit-learn.org/1.4/modules/generated/sklearn.manifold.TSNE.html). Other historical API or execution-state issues may still need attention.

## Dataset attribution

The data is Breast Cancer Wisconsin (Diagnostic), UCI dataset 17, DOI `10.24432/C5DW2B`. Its source page provides authorship, feature information and dataset terms. [UCI dataset record](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic).

No separate dataset or trained-model artifact was supplied in the original coursework folder. The notebook contains the fetching code and saved analysis outputs. No software licence is assigned by this packaging step.

## What was verified

The repository check validates Python syntax, notebook format, JSON and tracked documentation links. It does not execute notebooks or evaluate classifiers. File hashes additionally confirm that the notebook, report and local recording match the supplied artifacts.
