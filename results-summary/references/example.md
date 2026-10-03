# Worked examples: interpreting classification results

These examples use illustrative experimental evidence, not results from a real project. Use them to calibrate judgment and distinguish the modes, not as required report templates. The first two responses inspect the same evidence; only the user's request changes. The final example shows a critique that supports the user's interpretation.

## Shared evidence: overall improvement with uneven performance

Assume the following artifacts were supplied and inspected:

- `evaluation_config.yaml`: Two representations are compared using the same logistic regression evaluation procedure. Representation learning, scaling, classifier fitting, and hyperparameter selection use training and validation subjects only. Test subjects A, B, and C are held out until final evaluation. One training seed is available for each representation.
- `split_manifest.csv`: Training, validation, and test subject identifiers do not overlap.
- `subject_metrics.csv`: Correct predictions and cell counts are shown below. Overall accuracy pools cells across the three test subjects.
- `class_metrics.csv`: Of the 1,000 test cells, 900 belong to the majority cell type and 100 to the minority cell type. The baseline correctly classifies 40 of the 100 minority cells; the new representation correctly classifies 20.

| Test subject | Cells | Baseline correct | New correct | Baseline accuracy | New accuracy |
|---|---:|---:|---:|---:|---:|
| A | 800 | 720 | 776 | 90% | 97% |
| B | 100 | 70 | 60 | 70% | 60% |
| C | 100 | 70 | 60 | 70% | 60% |
| Pooled | 1,000 | 860 | 896 | 86% | 89.6% |

### `--mode scan`

#### Invocation

```text
$results-summary --mode scan
Look through evaluation_config.yaml, split_manifest.csv,
subject_metrics.csv, and class_metrics.csv. What should I take away?
```

#### Example response

The new representation improves pooled accuracy from 86% to 89.6%, but that improvement is concentrated in the largest test subject. Performance worsens on the other two subjects and on the minority cell type.

- In `subject_metrics.csv`, subject A improves from 90% to 97% and supplies 80% of the test cells. Subjects B and C each drop from 70% to 60%. Giving each subject equal weight changes the comparison: mean subject accuracy falls from 76.7% to 72.3%.
- In `class_metrics.csv`, minority-class recall falls from 40% to 20%. The overall gain therefore does not describe an improvement across both cell types.
- The supplied configuration and split manifest indicate that test subjects are excluded from training and model selection. They show no subject overlap, although configuration alone cannot establish that every preprocessing step followed the intended split.

The main next analysis is a confusion matrix for each subject, with class counts. That would help distinguish differences in class composition from differences in classification within each class. The current summaries do not establish why subjects B and C deteriorate. Repeating the matched comparison across training seeds would then show whether this pattern is stable; only one seed per representation is available here.

### `--mode critique`

#### Invocation

```text
$results-summary --mode critique
The improvement from 86% to 89.6% shows that the new representation
generalizes better across subjects. Evaluate that interpretation using
evaluation_config.yaml, split_manifest.csv, subject_metrics.csv,
and class_metrics.csv.
```

#### Example response

The results support higher pooled accuracy on these held-out cells, but do not support a general improvement across subjects. Two of the three test subjects perform worse with the new representation.

The distinction comes from how accuracy is aggregated. In `subject_metrics.csv`, subject A provides 80% of the cells and improves by 7 percentage points, outweighing the 10-point declines on both B and C. Equal weighting of subjects instead gives 76.7% for the baseline and 72.3% for the new representation. Neither aggregation is inherently wrong: pooled accuracy describes the observed mixture of cells, while equal subject weighting answers how performance compares for an equally weighted set of these subjects.

The minority-class recall decline from 40% to 20% in `class_metrics.csv` further limits the claim. Class composition could contribute to the subject-level pattern, but the supplied summaries do not establish that explanation. The split manifest and configuration support an intended held-out-subject evaluation; this is a limitation of the improvement claim, not evidence that the test subjects were used for training.

A supported interpretation would be: “The new representation improves pooled accuracy on this test set, driven by the largest subject, while accuracy on the other two subjects and minority-class recall decline.”

To assess a broader generalization claim, first compare per-subject confusion matrices and class counts, then repeat the matched evaluation across training seeds. Consistent improvements across subjects and classes would strengthen the claim; persistence of the current pattern would favor a narrower conclusion. Additional independent test subjects would be needed to assess how widely that conclusion holds.
.
