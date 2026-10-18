# DSCI 552 - Homework 4

Vishnu Gamini · GitHub: vishnugamini

Binary and multiclass activity classification using AReM time-series features, plus ISLR Section 4.8 exercises 3 and 7.

## Files

```text
data/
  AReM/
    bending1/   bending2/   cycling/   lying/   sitting/   standing/   walking/
    bendingType.pdf
    sensorsPlacement.pdf
  results/
    features_l1.csv
    binary_rfe_cv.csv
    binary_rfe_coefficients.csv
    case_control_coefficients.csv
    binary_l1_cv.csv
    binary_comparison.csv
    binary_metrics.csv
    binary_test_probabilities.csv
    multinomial_logistic_cv.csv
    naive_bayes_cv.csv
    multiclass_comparison.csv
    multiclass_auc.csv
    l1_multinomial_test_probabilities.csv
    gaussian_nb_test_probabilities.csv
    multinomial_nb_test_probabilities.csv
    selected_models.json
notebook/
  Gamini_Vishnu_HW4.ipynb
requirements.txt
```

## Run locally

Use Python 3.12. From the repository root:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m notebook
```

Open `notebook/Gamini_Vishnu_HW4.ipynb`, restart the Python kernel, and run all cells. All data are included, paths are relative, and outputs are saved. The notebook recreates the result files; no network connection is needed to execute it.

## Methods

The assignment's file-based split gives 69 training and 19 test recordings. The Homework 3 feature choices for scatterplots are mean, sample standard deviation, and maximum. Modeling extracts all seven statistics from each of six series and each of 1–20 consecutive segments, yielding 42–840 candidate features per recording.

Binary selection uses five stratified folds. RFE searches retained feature counts 1, 2, 3, 5, 10, 20, and 40 with a large logistic-regression budget C = 1,000,000. Binary L1 searches C from 0.001 through 1,000 in powers of ten. Multiclass selection uses four stratified folds because bending2 has four training recordings. Multinomial L1 searches C = 0.01, 0.1, 1, and 10. Gaussian and multinomial naive Bayes tune smoothing and segmentation. Scaling and feature selection are fitted within training folds. Test recordings do not participate in model selection. Random seeds are 42.

RFE uses the assignment's permitted alternative to p-value selection. The notebook reports approximate Wald p-values and checks complete separation. A balanced case-control sample is fitted separately, with its intercept corrected to the original training class prevalence. Both Gaussian and multinomial naive Bayes use empirical class priors; their names describe the conditional feature distributions. Multinomial naive Bayes is an approximation for the continuous, nonnegative statistics here.

## Data and cleaning

The original UCI files are preserved. Parsing handles space-separated rows in `bending2/dataset4.csv` and final trailing commas in `cycling/dataset9.csv` and `cycling/dataset14.csv`. The single missing reading at 13,500 ms in `sitting/dataset8.csv` is interpolated from its neighbors, as in Homework 3, to give 480 readings per cleaned recording.

Palumbo, F., Gallicchio, C., Pucci, R., and Micheli, A. (2016). *Activity Recognition system based on Multisensor data fusion (AReM)*. UCI Machine Learning Repository. https://doi.org/10.24432/C5SS33. Dataset license: CC BY 4.0.

Textbook: James, G., Witten, D., Hastie, T., and Tibshirani, R. (2021). *An Introduction to Statistical Learning*, second edition, Section 4.8, exercises 3 and 7. https://www.statlearning.com/.
