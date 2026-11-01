# Calibration and uncertainty-aware policy evaluation — v2 actual results
Generated 2026-11-01T06:08:21.720375+00:00. Evaluation hash: a0f684269a251ad27ae629b067cbcf7ee910ecadfe466268878a75a86fd0c979. Frozen decision hash: 74cf956e0564444a1c87a6eac7eb76c1dd129dcbb8f1c756d3f171ba51068058.
Experiment: five_categories_camera_balanced_v2. Training ID: training_v2_corrected. Evaluated all 6 planned checkpoints.

## Calibration (separate camera partition)
Each corrected Notebook 03 v2 selected head-or-fine-tuning checkpoint is kept unchanged. One positive scalar temperature per run fitted by minimizing **image-weighted calibration NLL** within [0.01, 100.0]. Calibration fitting does not alter the CNN or argmax labels. The calibrated probability quality is evaluated independently on the held-out test split.

## Binomial-bound threshold selection (secondary analysis; policy cameras only)
For each raw and scaled model/seed output, compute confidence as the maximum softmax probability. Accept if confidence >= threshold and abstain on the rest; no manual image review is performed in this experiment. We fixed 85 cutoffs in the interval [0.2, 1.0] before reading policy outcomes, including a finer high-confidence tail. Candidates are not extracted from policy observations. Ties are accepted together.

For each candidate, observe n accepted policy images and e wrong predictions, and compute a one-sided exact Clopper–Pearson 95%-familywise *binomial* upper bound on its unknown accepted error. Bonferroni uses alpha = 0.05/(12 policies x 85 cutoffs) = 4.90196078e-05 per test. Binomial exactness assumes independent observations at a fixed cutoff. The multiple-testing adjustment allows threshold selection among all fixed cutoffs under that independence model.

For camera diagnostics only, we also ran 10000 seeded whole-policy-camera resamples over 13 distinct policy cameras, shared across all policies. We computed an approximate standardized-bootstrap critical value 3.78570 from the largest standardized positive bootstrap deviation across eligible cutoffs and policies, then formed per-cutoff approximate simultaneous camera-bootstrap upper estimates only where nondegenerate evidence exists. Zero observed errors, zero/near-zero variation and nonfinite estimates imply an unavailable camera diagnostic, not zero camera uncertainty and not rejection of the threshold. All images in a sampled camera stay together, and error/coverage remain image-weighted within the sampled mixture. Bootstrap replicates with no accepted images retain the original 100%-risk assignment in this diagnostic. Neither camera-estimate availability nor its value is used in selection.

The selection criterion uses only **binomial_bonferroni_ucb_95 <= 5%**, with >= 100 accepted images and >= 5 contributing cameras. Zero-error candidates are allowed when this bound passes. For schema compatibility, screening_upper is an alias of the binomial bound. Camera-bootstrap estimates remain separate diagnostics and never veto a candidate. Among passing candidates, choose the greatest number of accepted images; tie-break by the higher cutoff. No passing cutoff means no automatic predictions (undefined accepted error), not zero-error success. 12/12 model/seed/variant policies selected a cutoff on the v2 policy data. Each candidate's observed risk and both upper bounds are available in its policy_candidates CSV. Raw and scaled policies were selected separately. All selections and temperatures were frozen before inference on test images.

**Interpretation of uncertainty:** The Clopper–Pearson bound is exact for independent accepted images and fixed cutoffs (with Bonferroni protecting the fixed-grid search), but the events are clustered within cameras. The camera-bootstrap max-t upper bound acknowledges camera structure, but with a modest number of cameras it is *approximate*, not an exact finite-sample interval. Selection by the binomial bound is assumption-dependent, **not a mathematical guarantee of <=5% errors at a newly deployed camera**. A missing camera diagnostic remains explicitly missing even when a threshold is selected; a finite diagnostic is not proof of valid camera-aware coverage. All risk estimates describe the available selected event/camera distributions; shifts in camera mix, animal class prevalence and unseen locations can defeat transfer. A policy with no threshold passing this screening rule may indicate insufficient evidence rather than a poor classifier.

## Temperatures (fitted development split)
```text
             model  seed  temperature  calibration_nll_raw  calibration_nll_scaled  at_temperature_boundary
mobilenet_v3_large    42       1.3145               0.6153                  0.5983                    False
   efficientnet_b0    42       1.1619               0.5928                  0.5870                    False
mobilenet_v3_large   123       1.8638               0.6541                  0.5671                    False
   efficientnet_b0   123       1.7828               0.6120                  0.5295                    False
mobilenet_v3_large  2026       1.6721               0.6965                  0.6273                    False
   efficientnet_b0  2026       1.0336               0.6060                  0.6057                    False
```

## Threshold decisions (policy split, with upper confidence bounds)
```text
             model  seed variant  temperature   status                               reason  threshold  accepted  errors  accepted_cameras  coverage  accepted_error  binomial_bonferroni_ucb_95  camera_uncertainty_available      camera_uncertainty_reason  camera_bootstrap_degenerate  camera_bootstrap_sd  camera_bootstrap_simultaneous_upper_approx  screening_upper
mobilenet_v3_large    42     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9600       737      14                12    0.4913          0.0190                      0.0468                          True approximate_estimate_available                        False               0.0069                                      0.0452           0.0468
mobilenet_v3_large    42  scaled       1.3145 selected passes_binomial_bonferroni_screening     0.9000       744      15                12    0.4960          0.0202                      0.0483                          True approximate_estimate_available                        False               0.0077                                      0.0492           0.0483
   efficientnet_b0    42     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9100       855      17                13    0.5700          0.0199                      0.0455                          True approximate_estimate_available                        False               0.0075                                      0.0481           0.0455
   efficientnet_b0    42  scaled       1.1619 selected passes_binomial_bonferroni_screening     0.8600       875      20                13    0.5833          0.0229                      0.0493                          True approximate_estimate_available                        False               0.0079                                      0.0527           0.0493
mobilenet_v3_large   123     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9900       784      14                13    0.5227          0.0179                      0.0440                          True approximate_estimate_available                        False               0.0113                                      0.0605           0.0440
mobilenet_v3_large   123  scaled       1.8638 selected passes_binomial_bonferroni_screening     0.8900       799      17                13    0.5327          0.0213                      0.0486                          True approximate_estimate_available                        False               0.0126                                      0.0690           0.0486
   efficientnet_b0   123     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9600       940      21                13    0.6267          0.0223                      0.0474                          True approximate_estimate_available                        False               0.0082                                      0.0536           0.0474
   efficientnet_b0   123  scaled       1.7828 selected passes_binomial_bonferroni_screening     0.8200       931      19                13    0.6207          0.0204                      0.0449                          True approximate_estimate_available                        False               0.0076                                      0.0490           0.0449
mobilenet_v3_large  2026     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9900       602       8                12    0.4013          0.0133                      0.0419                          True approximate_estimate_available                        False               0.0069                                      0.0393           0.0419
mobilenet_v3_large  2026  scaled       1.6721 selected passes_binomial_bonferroni_screening     0.9100       634      11                13    0.4227          0.0174                      0.0472                          True approximate_estimate_available                        False               0.0083                                      0.0489           0.0472
   efficientnet_b0  2026     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9400       665      11                13    0.4433          0.0165                      0.0451                          True approximate_estimate_available                        False               0.0090                                      0.0505           0.0451
   efficientnet_b0  2026  scaled       1.0336 selected passes_binomial_bonferroni_screening     0.9300       670      12                13    0.4467          0.0179                      0.0470                          True approximate_estimate_available                        False               0.0094                                      0.0535           0.0470
```

## Held-out test evaluation
3000 distinct event representatives from 28 previously excluded classifier camera locations. The five categories and camera-specific sample sizes come from the frozen v2 data selection. Accuracy, five-class macro-F1, NLL, summed multiclass Brier, 15-bin top-label ECE (10/20-bin sensitivity), coverage and accepted-error are reported without test-driven changes. If a policy accepts no images, accepted error and empirical test-target status remain undefined.

```text
             model  seed variant  temperature  images  cameras  threshold policy_status  policy_accepted_error  policy_screening_upper    nll  accuracy  macro_f1  brier    ece  accepted  accepted_errors  coverage  manual_review_fraction  accepted_error  accepted_cameras  target_met_on_test
mobilenet_v3_large    42     raw       1.0000    3000       28     0.9600      selected                 0.0190                  0.0468 0.4676    0.8243    0.7236 0.2351 0.0288      1596               21    0.5320                  0.4680          0.0132                27                True
mobilenet_v3_large    42  scaled       1.3145    3000       28     0.9000      selected                 0.0202                  0.0483 0.4611    0.8243    0.7236 0.2344 0.0241      1632               22    0.5440                  0.4560          0.0135                27                True
   efficientnet_b0    42     raw       1.0000    3000       28     0.9100      selected                 0.0199                  0.0455 0.4177    0.8373    0.7368 0.2156 0.0184      1727               11    0.5757                  0.4243          0.0064                26                True
   efficientnet_b0    42  scaled       1.1619    3000       28     0.8600      selected                 0.0229                  0.0493 0.4260    0.8373    0.7368 0.2170 0.0326      1754               13    0.5847                  0.4153          0.0074                26                True
mobilenet_v3_large   123     raw       1.0000    3000       28     0.9900      selected                 0.0179                  0.0440 0.3820    0.8743    0.7998 0.1819 0.0364      1843                9    0.6143                  0.3857          0.0049                26                True
mobilenet_v3_large   123  scaled       1.8638    3000       28     0.8900      selected                 0.0213                  0.0486 0.3627    0.8743    0.7998 0.1804 0.0367      1856               10    0.6187                  0.3813          0.0054                26                True
   efficientnet_b0   123     raw       1.0000    3000       28     0.9600      selected                 0.0223                  0.0474 0.3852    0.8670    0.7856 0.1849 0.0446      2133               38    0.7110                  0.2890          0.0178                28                True
   efficientnet_b0   123  scaled       1.7828    3000       28     0.8200      selected                 0.0204                  0.0449 0.3602    0.8670    0.7856 0.1796 0.0303      2123               39    0.7077                  0.2923          0.0184                28                True
mobilenet_v3_large  2026     raw       1.0000    3000       28     0.9900      selected                 0.0133                  0.0419 0.4412    0.8383    0.7587 0.2250 0.0385      1414                9    0.4713                  0.5287          0.0064                26                True
mobilenet_v3_large  2026  scaled       1.6721    3000       28     0.9100      selected                 0.0174                  0.0472 0.4338    0.8383    0.7587 0.2237 0.0378      1509               14    0.5030                  0.4970          0.0093                26                True
   efficientnet_b0  2026     raw       1.0000    3000       28     0.9400      selected                 0.0165                  0.0451 0.4765    0.8247    0.7291 0.2416 0.0219      1452               13    0.4840                  0.5160          0.0090                26                True
   efficientnet_b0  2026  scaled       1.0336    3000       28     0.9300      selected                 0.0179                  0.0470 0.4769    0.8247    0.7291 0.2418 0.0229      1470               14    0.4900                  0.5100          0.0095                26                True
```

## Seed summaries and category-conditioned acceptance
Accepted-error summaries are means among seeds with acceptance, with the contributing seed count,
abstaining seed count, and selected-policy seed count shown explicitly. Undefined errors remain missing.
```text
             model variant  seed_count  accepted_error_contributing_seeds  abstaining_seeds  seeds_with_selected_policy  accepted_error_mean_among_accepting_seeds  accepted_error_std_among_accepting_seeds  accuracy_mean  accuracy_std  macro_f1_mean  macro_f1_std  nll_mean  nll_std  brier_mean  brier_std  ece_mean  ece_std  coverage_mean  coverage_std
   efficientnet_b0     raw           3                                  3                 0                           3                                     0.0110                                    0.0060         0.8430        0.0217         0.7505        0.0306    0.4264   0.0463      0.2141     0.0284    0.0283   0.0142         0.5902        0.1142
   efficientnet_b0  scaled           3                                  3                 0                           3                                     0.0118                                    0.0058         0.8430        0.0217         0.7505        0.0306    0.4210   0.0585      0.2128     0.0313    0.0286   0.0050         0.5941        0.1091
mobilenet_v3_large     raw           3                                  3                 0                           3                                     0.0081                                    0.0044         0.8457        0.0258         0.7607        0.0381    0.4303   0.0438      0.2140     0.0283    0.0346   0.0051         0.5392        0.0718
mobilenet_v3_large  scaled           3                                  3                 0                           3                                     0.0094                                    0.0040         0.8457        0.0258         0.7607        0.0381    0.4192   0.0508      0.2128     0.0286    0.0329   0.0076         0.5552        0.0586
```
Accepted counts/errors, support, coverage and error rate are reported separately by true and predicted
category in test_accepted_by_category.csv. The full camera audit is deferred to Notebook 05 v2.
Manual crop verification was not performed in this experiment; automatic integrity checks do not
establish semantic crop-label agreement.

## Primary paired NLL calibration-gain comparison
Gain is raw minus scaled NLL. The architecture comparison is EfficientNet gain minus MobileNet gain,
averaged across all three paired seeds in each shared whole-test-camera resample. NLL is primary;
this comparison was fixed before test inference. Positive values favour a larger EfficientNet gain,
negative values a larger MobileNet gain. A zero-crossing interval is inconclusive, not equivalence.
```text
                                                        comparison metric  primary_metric  estimate  ci_low  ci_high  valid_replicates  total_replicates
        mobilenet_v3_large NLL gain (raw minus scaled; mean seeds)    nll            True    0.0111 -0.0044   0.0462              2000              2000
           efficientnet_b0 NLL gain (raw minus scaled; mean seeds)    nll            True    0.0054 -0.0055   0.0294              2000              2000
EfficientNet NLL gain minus MobileNet NLL gain (mean paired seeds)    nll            True   -0.0057 -0.0219   0.0018              2000              2000
```

## Held-out test uncertainty
2000 paired whole-test-camera bootstrap draws with seed 20261026 form descriptive two-sided percentile 95% intervals and architecture/temperature contrasts. Same test cameras are used in all model/seed replicates. The estimates condition on model weights, calibration temperatures and selected thresholds; they do not include retraining, split selection, or temperature/threshold refitting uncertainty. Policy-camera upper bounds are NOT the test intervals. Training seed means/std reflect a fixed camera split.

## Scope and limitations
Classifier training and selection follow the v2 one-frame-per-eligible-event, camera-stratified, development-camera-capped pipeline. The camera counts and class distributions were not copied from v1. Notebook 02 v2 uses official precomputed MDv5a animal boxes and full-frame fallback, so species-label or crop semantic correctness is not guaranteed. Detector training may have included Caltech images; camera holdout refers to downstream classifier training, not detector pretraining. Rare categories, differing camera environments and the cropped/full-frame mixture can affect performance. Training head/fine-tuning choices and checkpoint selection remain as frozen in Notebook 03 v2. Results cannot isolate architecture from its particular pretrained weights. No claim of verified future error control is made.

## Outputs and references
Tables: temperature_summary.csv, policy_summary.csv, twelve policy_candidates_*.csv, test_results.csv, test_seed_summary.csv, test_per_class.csv, test_accepted_by_category.csv, calibration_gain_comparison.csv, ece_bin_sensitivity.csv, test_camera_bootstrap_intervals.csv, paired_camera_bootstrap_contrasts.csv. Figures: test_reliability.png, test_risk_coverage.png, test_confusion_matrices.png. Audit files: evaluation_protocol.json, frozen_calibration_and_policies.json, test_exports.json. Each test-prediction creation hash is journalled immediately and checked before cache reuse; existing hashes are never re-baselined. Completed evaluations anchor the registry hash in completion.json. Input/provenance checks use explicit exceptions, not removable assertions. Model/logit files are under models/five_categories_camera_balanced_v2/evaluation_v2 and the training_v2_corrected directories.

Guo et al. (2017), On Calibration of Modern Neural Networks: https://proceedings.mlr.press/v70/guo17a.html
Clopper & Pearson (1934), The Use of Confidence or Fiducial Limits Illustrated in the Case of the Binomial: https://doi.org/10.1093/biomet/26.4.404
Hoeffding/Bonferroni style multiple-threshold selection and limitations: describe the fixed grid and statistical assumptions above; no external selective-risk guarantee is asserted for this clustered setup.
PyTorch weights-only loading: https://docs.pytorch.org/docs/stable/generated/torch.load.html
MegaDetector results: https://lila.science/megadetector-results-for-camera-trap-datasets/
