# Writing briefs for Sections 5 to 7

What each section of the report should cover. Take every number from the final notebook run, write in first person plural, and do not copy this text into the report.

## 5. Choice and Description of the Algorithms

[AVERY - Sections 5.1 to 5.5 and 5.7, minimum 1,000
words. Write in first person plural (we), active voice, no long dashes.
Take every number from the final notebook run, not from the Assessment 3
slides. Delete all yellow text before export.]

### 5.1 Why these models

[One paragraph. We chose models of increasing
capacity so the comparison means something: an interpretable baseline
(Decision Tree), an ensemble (Random Forest), a probabilistic model with
a strong assumption (Naive Bayes), a feed-forward network (ANN) and a
sequence model (LSTM). Say that all five receive the same split and the
same class weighting.]

### 5.2 Decision Tree

[How a tree splits (Gini impurity). Our settings:
max_depth 10, min_samples_leaf 20, class_weight balanced, and why depth
is limited. Include its confusion matrix as a figure and state what it
shows: how many false alarms and how many missed anomaly
minutes.]

### 5.3 Random Forest

[Bagging and feature subsampling in two or three
sentences. Settings: 200 trees, max_depth 15, min_samples_leaf 5.
Include the Gini importance chart and interpret it. Important: Gini
importance ranks sensor_00 first, but permutation importance (Section 7)
does not. Say that the two measures differ and why (impurity importance
is computed on training data and is biased towards correlated
features).]

### 5.4 Naive Bayes

[State the conditional independence assumption. Link
to Section 4.5: 51 sensors collapse to about 28 groups, so the same
signal is counted several times and the model becomes over-confident.
That is the mechanism behind its lower precision. Do not write that all
sensors are correlated: mean correlation is only 0.27.]

### 5.5 Artificial Neural Network

[Architecture: Dense 64, Dropout 0.3, Dense 32,
Dropout 0.2, sigmoid output. Adam, binary cross-entropy, early stopping
on validation AUC, class weights, seed set immediately before the model
is built. Include the training curve and say what it shows about
overfitting.]

### 5.6 Long Short-Term Memory network

[JANE. Why a sequence model: it reads a 20-minute
window of the 50 raw sensors instead of one row. Architecture: LSTM 64,
Dropout, LSTM 32, Dropout, Dense 16, sigmoid (Hochreiter & Schmidhuber,
1997). Training set built by undersampling normal windows to 30,000; the
test set is left at its true imbalance. Include the
predicted-probability-over-time figure and interpret it.]

### 5.7 Hyperparameter tuning with GridSearchCV

[AVERY. New section 15 of the notebook, from Week
10. Describe the grid (Decision Tree: max_depth and min_samples_leaf;
Random Forest: n_estimators, max_depth, min_samples_leaf), 3-fold
stratified cross-validation, scoring on PR-AUC, searched on 40,000
training rows and refitted on the full training split. Insert the
comparison table from the notebook. Discuss three findings: (1) the
tuned tree is shallower than our hand-picked one and improves PR-AUC,
but check what happens to F1 at the 0.5 threshold; (2) the hand-picked
Random Forest was already near the best configuration; (3) the
cross-validated score is higher than the test score because shuffled
folds put neighbouring minutes on both sides (Bergmeir & Benítez, 2012),
so the chronological test result is the one to trust.]

## 6. Overview of Model Evaluation Measures

[JANE - Sections 5.6, 6 and 7, minimum 1,000 words.
Same style rules as above. Delete all yellow text before
export.]

### 6.1 The confusion matrix

[Define TP, FP, FN, TN in terms of this problem: a
false negative is a missed anomaly minute, a false positive is a false
alarm. A small labelled 2 x 2 figure helps.]

### 6.2 Accuracy, precision, recall and F1

[Give each formula. Then the key argument:
predicting NORMAL for every row gives 93.43% accuracy and zero recall,
so accuracy cannot rank these models. Explain the precision and recall
trade-off for maintenance: a missed failure costs more than an
unnecessary inspection, but too many false alarms cause alarm
fatigue.]

### 6.3 ROC-AUC and PR-AUC

[Both are threshold-independent. Explain why PR-AUC
is stricter under imbalance (Saito & Rehmsmeier, 2015): its baseline is
the positive rate, 0.062, not 0.5.]

### 6.4 Decision thresholds

[Precision, recall and F1 depend on the threshold;
ROC-AUC and PR-AUC do not. State our rule: thresholds are selected on a
validation window (the last 10% of the training timeline) and only then
applied to the test set. Selecting on the test set would overstate
performance.]

## 7. Results and Overall Understanding

### 7.1 Results at the default threshold

[JANE. Paste the first table printed by the cell
above the notebook conclusion. Report the accuracy range and the PR-AUC
range and the ratio between them, which the same cell prints. Include
the combined ROC and PR curve figure and interpret it.]

### 7.2 Results at validation-selected thresholds

[Paste the second table. Discuss which models gain
from threshold selection and which do not. For the LSTM, report both its
default and its selected threshold result, and say that its ranking
metrics are stable while its default-threshold precision is
not.]

### 7.3 Diagnostics

[Calibration (Brier scores), permutation importance
(report the top feature for Random Forest and for the ANN from the
ranked tables, and note that sensor_00 is not the top feature under
permutation importance), per-episode detection (all three test episodes
detected by every model), and training time.]

### 7.4 Limitations and future work

[Only seven failures. One split date. Detection, not
prediction (link to Section 4.4). Thresholds chosen for F1 treat
precision and recall equally, which maintenance costs do not. A second
normal operating mode exists (Section 4.5), so a deployed model needs
retraining. Future work: prediction from variability features (Section
4.3), walk-forward validation.]

### 7.5 Overall understanding

[Two paragraphs that answer the project question
plainly: which model we would deploy and why, and the three lessons we
would carry into another project (check the split for label content,
judge by PR-AUC not accuracy, choose thresholds on validation
data).]

**References**

Bergmeir, C., & Benítez, J. M. (2012). On the use of cross-validation
for time series predictor evaluation. *Information Sciences*, *191*,
192-213. https://doi.org/10.1016/j.ins.2011.12.028

Carvalho, T. P., Soares, F. A. A. M. N., Vita, R., Francisco, R. da P.,
Basto, J. P., & Alcalá, S. G. S. (2019). A systematic literature review
of machine learning methods applied to predictive maintenance.
*Computers & Industrial Engineering*, *137*, Article 106024.
https://doi.org/10.1016/j.cie.2019.106024

Davies, D. L., & Bouldin, D. W. (1979). A cluster separation measure.
*IEEE Transactions on Pattern Analysis and Machine Intelligence*,
*PAMI-1*(2), 224-227. https://doi.org/10.1109/TPAMI.1979.4766909

He, H., & Garcia, E. A. (2009). Learning from imbalanced data. *IEEE
Transactions on Knowledge and Data Engineering*, *21*(9), 1263-1284.
https://doi.org/10.1109/TKDE.2008.239

Hochreiter, S., & Schmidhuber, J. (1997). Long
short-term memory. *Neural Computation*, *9*(8), 1735-1780.
https://doi.org/10.1162/neco.1997.9.8.1735

Kaufman, S., Rosset, S., Perlich, C., & Stitelman, O. (2012). Leakage in
data mining: Formulation, detection, and avoidance. *ACM Transactions on
Knowledge Discovery from Data*, *6*(4), Article 15.
https://doi.org/10.1145/2382577.2382579

Nphantawee. (2018). *Pump sensor data* [Data set]. Kaggle.
https://www.kaggle.com/datasets/nphantawee/pump-sensor-data

Pedregosa, F., Varoquaux, G., Gramfort, A., Michel, V., Thirion, B.,
Grisel, O., Blondel, M., Prettenhofer, P., Weiss, R., Dubourg, V.,
Vanderplas, J., Passos, A., Cournapeau, D., Brucher, M., Perrot, M., &
Duchesnay, E. (2011). Scikit-learn: Machine learning in Python. *Journal
of Machine Learning Research*, *12*, 2825-2830.

Rousseeuw, P. J. (1987). Silhouettes: A graphical aid to the
interpretation and validation of cluster analysis. *Journal of
Computational and Applied Mathematics*, *20*, 53-65.
https://doi.org/10.1016/0377-0427(87)90125-7

Saito, T., & Rehmsmeier, M. (2015). The precision-recall plot is more
informative than the ROC plot when evaluating binary classifiers on
imbalanced datasets. *PLoS ONE*, *10*(3), Article e0118432.
https://doi.org/10.1371/journal.pone.0118432

[The highlighted Hochreiter reference is cited only
in Section 5.6. Keep it once that section is written. Add any other
source Avery or Jane cite, in APA 7 and in alphabetical order, and
remove any reference that ends up uncited.]
