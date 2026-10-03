# Worked example: cell-type classification across subjects

The examples below summarize the same illustrative source file at three detail levels. Use them to calibrate depth, not as a required report template. The source defines an evaluation procedure; it does not include experimental results.

## Shared source: `evaluate_cell_types.py`

```python
import numpy as np
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler


def evaluate_cell_types(features, labels, subject_ids):
    """Predict cell types for held-out subjects.

    Args:
        features: Numeric array of shape (n_cells, n_features).
        labels: Cell-type labels of shape (n_cells,).
        subject_ids: Subject identifiers of shape (n_cells,).

    Returns:
        A dictionary containing one prediction per input cell and pooled
        classification accuracy across all cells.
    """
    features = np.asarray(features)
    labels = np.asarray(labels)
    subject_ids = np.asarray(subject_ids)
    predictions = np.empty_like(labels)

    for subject in np.unique(subject_ids):
        test_mask = subject_ids == subject
        train_mask = ~test_mask

        model = make_pipeline(
            StandardScaler(),
            LogisticRegression(max_iter=1000),
        )
        model.fit(features[train_mask], labels[train_mask])
        predictions[test_mask] = model.predict(features[test_mask])

    return {
        "predictions": predictions,
        "accuracy": float(np.mean(predictions == labels)),
    }
```

## `--detail surface`

### Invocation

```text
$code-summary evaluate_cell_types.py --detail surface
```

### Example response

This code evaluates whether measured neural features can distinguish cell types in previously unseen subjects. It repeatedly trains a classifier on all but one subject and tests it on the remaining subject, until every subject has been held out once.

It produces a predicted cell type for each cell and an overall classification accuracy. The evaluation measures generalization across subjects, assuming the features and cell-type labels are comparable across recordings.

## `--detail medium`

### Invocation

```text
$code-summary evaluate_cell_types.py --detail medium
```

### Example response

The code evaluates cell-type classification with leave-one-subject-out validation. It takes cell-level features, labels, and subject identifiers, and returns one held-out prediction per cell plus pooled accuracy.

Each fold fits a fresh logistic regression classifier. Features are standardized using only the training subjects, and the same transformation is applied to the held-out subject. This prevents the held-out subject from influencing the scaling in this function; any preprocessing used to construct the input features needs separate consideration.

The final accuracy pools cells rather than averaging subject-level scores. Subjects with more cells therefore contribute more, and accuracy can obscure poor performance on rare cell types. The code does not report per-subject or per-class performance.

The evaluation assumes aligned inputs, consistent feature and label definitions, and subject identifiers that correctly group cells from the same subject. Every training fold must contain at least two cell types for logistic regression to fit. This source establishes the evaluation procedure, but contains no results showing how well the features generalize.

## `--detail deep`

### Invocation

```text
$code-summary evaluate_cell_types.py --detail deep
```

### Example response

`evaluate_cell_types` performs leave-one-subject-out cell-type classification using standardized features and logistic regression. Each cell is predicted by a model trained on other subjects, and the function combines those predictions into a single accuracy score.

**Inputs and assumptions.** `features` is a numeric array with shape `(n_cells, n_features)`; `labels` and `subject_ids` each have shape `(n_cells,)`. Corresponding rows must describe the same cell. The function converts all three inputs with `np.asarray`, but does not explicitly check their dimensions, alignment, or missing values. It assumes consistent feature definitions and cell-type labels across subjects. Subject identifiers must be nonmissing and correctly group all cells belonging to the same subject.

**Evaluation flow.** `np.unique(subject_ids)` determines the held-out subjects. For each subject, an equality mask selects its test cells and the complementary mask selects training cells. A fresh pipeline fits `StandardScaler` followed by `LogisticRegression(max_iter=1000)` on the training subset. The scaler learns feature means and scales from that subset; prediction applies the fitted transformation to the test subset before classification. Neither fitted preprocessing nor classifier state is reused between folds.

This isolates subjects during model fitting and scaling within the function. It does not establish that upstream feature extraction was independent of held-out subjects or labels; that requires inspecting how `features` was produced.

**Model choices and dependencies.** NumPy provides array conversion, masking, and score aggregation. Scikit-learn supplies the preprocessing pipeline and classifier. The classifier sets an iteration limit of 1,000 and otherwise uses the installed library's defaults. The function performs no hyperparameter search, class reweighting, or feature selection. The iteration limit does not guarantee convergence, and the function does not inspect convergence warnings.

**Outputs and aggregation.** `predictions` is allocated with the shape and dtype of `labels`. Each fold writes its predictions into the original cells' positions, preserving input order. The returned dictionary contains this array and `accuracy`, a Python float computed as the number of correct predictions divided by the total number of cells. This is a cell-weighted score: subjects with more cells have more influence. Common cell types can also dominate accuracy. The function returns no per-subject scores, confusion matrix, probabilities, uncertainty estimates, or fitted models, and writes no files.

**Relevant edge cases.** A single subject leaves an empty training set. A training fold containing only one cell type also cannot fit logistic regression. A cell type present only in a held-out subject cannot be predicted by that fold's classifier because it was absent from training. The function provides no imputation for missing features; invalid inputs may fail inside NumPy or scikit-learn rather than with a targeted validation message. Empty inputs reach the mean of an empty array and produce `NaN` accuracy instead of a meaningful evaluation. Missing subject identifiers are particularly problematic: a `NaN` identifier does not equal itself, so its cells never receive held-out predictions and their allocated entries remain uninitialized.

The source shows how generalization is evaluated, but supplies no measured performance. Claims about predictive quality require the returned predictions and labels, with subject- and class-level analysis where relevant.
