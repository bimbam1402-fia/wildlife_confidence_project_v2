# Camera robustness audit — v2 actual results
Generated: 2026-11-01T06:28:56.401106+00:00. Experiment: five_categories_camera_balanced_v2; training: training_v2_corrected.
Parent evaluation hash: a0f684269a251ad27ae629b067cbcf7ee910ecadfe466268878a75a86fd0c979; frozen decisions: 74cf956e0564444a1c87a6eac7eb76c1dd129dcbb8f1c756d3f171ba51068058.

## Scope and provenance
This sensitivity analysis uses the existing v2 predictions, frozen temperatures and acceptance policies.
It does not retrain models, refit temperatures/thresholds, resplit data, download or inspect images,
or run new model inference. Corrected selected checkpoints may be head-only or fine-tuned; stage/epoch
and checkpoint digests are bound through the Notebook 03 index and Notebook 04 evaluation records.
There are 6 trained runs, 3000 test events, 28 represented test cameras,
and 3 paired training seeds. Manual crop verification was not performed; possible crop-label
mismatches remain a limitation. This is a v2 sensitivity analysis informed by the earlier study,
not a fresh independent test or proof of preregistration. Methods must not be revised using v2 outcomes.

## Camera and category representation
```text
      split  events  represented_cameras largest_camera  largest_camera_events  largest_share  top_three_share  concentration_equivalent_cameras  small_cameras  minimum_events  maximum_events
      train    4570                   64             70                    873        0.19103          0.49168                           9.56940             43               1             873
 validation    1500                   13             34                    300        0.20000          0.57133                           7.06725              6              14             300
calibration    1500                   13            115                    300        0.20000          0.50933                           7.68921              5               4             300
     policy    1500                   13             26                    300        0.20000          0.55200                           7.49321              3              10             300
       test    3000                   28             23                    896        0.29867          0.71233                           5.03263             17               1             896
```
```text
      split  coyote    deer     fox  rabbit   skunk
      train 0.26258 0.26258 0.10219 0.26258 0.11007
 validation 0.39800 0.27067 0.04333 0.24800 0.04000
calibration 0.43533 0.28067 0.03867 0.21067 0.03467
     policy 0.40733 0.26467 0.03867 0.24867 0.04067
       test 0.43567 0.25467 0.03600 0.24133 0.03233
```
Concentration-equivalent cameras is 1/sum(event_share squared), a count-concentration descriptor,
not a statistical effective sample size. Per-category concentration is recorded separately.
The audit describes the already-selected sample; it does not repair camera concentration or establish
that class/background changes caused generalization failures.

## Frozen policy outcomes and abstention
```text
             model variant   status  seeds
   efficientnet_b0     raw selected      3
   efficientnet_b0  scaled selected      3
mobilenet_v3_large     raw selected      3
mobilenet_v3_large  scaled selected      3
```
```text
             model  seed variant policy_status                        policy_reason  total_cameras  cameras_with_acceptance  cameras_without_acceptance                                                                                                                                                          accepting_camera_ids abstaining_camera_ids  accepted  accepted_errors  accepted_error
   efficientnet_b0    42     raw      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "37", "39", "4", "59", "69", "73", "76", "79", "80", "86", "91", "92", "96"]          ["33", "52"]      1727               11         0.00637
   efficientnet_b0    42  scaled      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "37", "39", "4", "59", "69", "73", "76", "79", "80", "86", "91", "92", "96"]          ["33", "52"]      1754               13         0.00741
   efficientnet_b0   123     raw      selected passes_binomial_bonferroni_screening             28                       28                           0 ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "73", "76", "79", "80", "86", "91", "92", "96"]                    []      2133               38         0.01782
   efficientnet_b0   123  scaled      selected passes_binomial_bonferroni_screening             28                       28                           0 ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "73", "76", "79", "80", "86", "91", "92", "96"]                    []      2123               39         0.01837
   efficientnet_b0  2026     raw      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "76", "79", "86", "91", "92", "96"]          ["73", "80"]      1452               13         0.00895
   efficientnet_b0  2026  scaled      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "76", "79", "86", "91", "92", "96"]          ["73", "80"]      1470               14         0.00952
mobilenet_v3_large    42     raw      selected passes_binomial_bonferroni_screening             28                       27                           1       ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "73", "76", "79", "86", "91", "92", "96"]                ["80"]      1596               21         0.01316
mobilenet_v3_large    42  scaled      selected passes_binomial_bonferroni_screening             28                       27                           1       ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "73", "76", "79", "86", "91", "92", "96"]                ["80"]      1632               22         0.01348
mobilenet_v3_large   123     raw      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "76", "79", "86", "91", "92", "96"]          ["73", "80"]      1843                9         0.00488
mobilenet_v3_large   123  scaled      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "76", "79", "86", "91", "92", "96"]          ["73", "80"]      1856               10         0.00539
mobilenet_v3_large  2026     raw      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "59", "69", "76", "79", "80", "86", "91", "92", "96"]          ["52", "73"]      1414                9         0.00636
mobilenet_v3_large  2026  scaled      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "59", "69", "76", "79", "80", "86", "91", "92", "96"]          ["52", "73"]      1509               14         0.00928
```
Each seed and camera with no accepted predictions has undefined accepted error, never zero error.
The camera_abstention_summary.csv and test_per_camera.csv files identify the affected cameras and seeds.
Thresholds follow Notebook 04's frozen binomial-bound selection and support requirements.
Camera uncertainty is diagnostic only: zero observed policy errors or an unavailable camera estimate
can coexist with a selected threshold. No-threshold outcomes are not treated as classification failure.
Classification/calibration metrics remain meaningful when no policy accepts.
No camera-weighted result certifies a new threshold or provides guaranteed camera-aware error control.

## Event and equal-camera weighting
```text
             model variant    weighting  accuracy     nll   brier     ece  macro_f1  coverage  accepted_error  seed_count  accepted_error_contributing_seeds  abstaining_seeds accepting_seed_ids abstaining_seed_ids  seeds_with_selected_policy  accepted_error_mean_among_accepting_seeds  accepted_error_std_among_accepting_seeds
   efficientnet_b0     raw equal_camera   0.79148 0.59919 0.29146 0.06411   0.78784   0.52011         0.02392           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.02392                                   0.03182
   efficientnet_b0     raw        image   0.84300 0.42644 0.21406 0.02829   0.75049   0.59022         0.01105           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.01105                                   0.00600
   efficientnet_b0  scaled equal_camera   0.79148 0.56356 0.28412 0.06527   0.78784   0.52087         0.02395           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.02395                                   0.03157
   efficientnet_b0  scaled        image   0.84300 0.42103 0.21282 0.02859   0.75049   0.59411         0.01177           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.01177                                   0.00581
mobilenet_v3_large     raw equal_camera   0.78496 0.59285 0.29364 0.07489   0.78504   0.48431         0.01633           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.01633                                   0.01245
mobilenet_v3_large     raw        image   0.84567 0.43027 0.21402 0.03457   0.76071   0.53922         0.00814           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.00814                                   0.00441
mobilenet_v3_large  scaled equal_camera   0.78496 0.55970 0.28356 0.06636   0.78504   0.49078         0.01620           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.01620                                   0.01218
mobilenet_v3_large  scaled        image   0.84567 0.41917 0.21284 0.03289   0.76071   0.55522         0.00938           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.00938                                   0.00405
```
Strict all-seed accepted-error means remain undefined if any seed has no acceptance. The separate
conditional mean among accepting seeds reports contributor counts and seed IDs explicitly, matching
Notebook 04's convention. All seeds contribute to coverage and classification/calibration summaries.
Event weighting describes this observed event mixture. Equal-camera weighting draws an observed
camera uniformly and an event uniformly within it; it does not imply representativeness of future sites.
Equal-camera pooled ECE/F1 are computed from camera-normalized additive statistics, not averages of
camera ECE/F1. Weighted accepted error is weighted mistakes divided by weighted accepted observations,
not the mean conditional error among accepting cameras. Those local diagnostics are labelled separately.

## Primary Notebook 04 NLL comparison verified
```text
                                                        comparison metric  matched  parent_estimate  audit_estimate   ci_low  ci_high  valid_replicates  total_replicates
        mobilenet_v3_large NLL gain (raw minus scaled; mean seeds)    nll     True          0.01110         0.01110 -0.00442  0.04622              2000              2000
           efficientnet_b0 NLL gain (raw minus scaled; mean seeds)    nll     True          0.00541         0.00541 -0.00554  0.02938              2000              2000
EfficientNet NLL gain minus MobileNet NLL gain (mean paired seeds)    nll     True         -0.00570        -0.00570 -0.02194  0.00181              2000              2000
```
Event-weighted NLL gains and the architecture difference, including paired bootstrap endpoints and
replicate counts, agree with Notebook 04. This reproduction is a consistency check, not independent
confirmation. NLL gain is raw minus scaled; the architecture contrast is EfficientNet gain minus
MobileNet gain. Positive means EfficientNet improved more; negative means MobileNet improved more.
The equal-camera contrast below is a sensitivity analysis. Brier/ECE are supporting outcomes.

## Camera-specific benefit and weighting sensitivity
```text
             model metric  cameras  improved  worsened  approximately_unchanged  equal_camera_mean_gain
mobilenet_v3_large    nll       28        18        10                        0                 0.03315
mobilenet_v3_large  brier       28        17        11                        0                 0.01008
mobilenet_v3_large    ece       28        11        17                        0                -0.01042
   efficientnet_b0    nll       28        17        11                        0                 0.03563
   efficientnet_b0  brier       28        17        11                        0                 0.00734
   efficientnet_b0    ece       28        13        15                        0                -0.00425
```
```text
                                            comparison    weighting metric  estimate   ci_low  ci_high  valid_replicates  total_replicates
mobilenet_v3_large calibration gain (raw minus scaled)        image    nll   0.01110 -0.00442  0.04622              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled)        image    nll   0.00541 -0.00554  0.02938              2000              2000
                EfficientNet gain minus MobileNet gain        image    nll  -0.00570 -0.02194  0.00181              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled)        image  brier   0.00119 -0.00320  0.01211              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled)        image  brier   0.00124 -0.00095  0.00700              2000              2000
                EfficientNet gain minus MobileNet gain        image  brier   0.00005 -0.00536  0.00349              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled)        image    ece   0.00168 -0.02078  0.02721              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled)        image    ece  -0.00030 -0.00896  0.01449              2000              2000
                EfficientNet gain minus MobileNet gain        image    ece  -0.00198 -0.01782  0.01410              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled) equal_camera    nll   0.03315 -0.00232  0.07416              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled) equal_camera    nll   0.03563  0.00548  0.07411              2000              2000
                EfficientNet gain minus MobileNet gain equal_camera    nll   0.00248 -0.02269  0.03439              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled) equal_camera  brier   0.01008 -0.00351  0.02588              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled) equal_camera  brier   0.00734  0.00026  0.01552              2000              2000
                EfficientNet gain minus MobileNet gain equal_camera  brier  -0.00275 -0.01328  0.00776              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled) equal_camera    ece   0.00854 -0.02186  0.02962              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled) equal_camera    ece  -0.00117 -0.01844  0.01805              2000              2000
                EfficientNet gain minus MobileNet gain equal_camera    ece  -0.00971 -0.03228  0.02204              2000              2000
```
Improved/worsened camera counts are descriptive; small-camera estimates are unstable. The
<30-event flag is not an exclusion rule or significance threshold.
Camera IDs represent dataset locations, not verified independent habitats or spatial distances.

## Influence of individual cameras
```text
             model    weighting  minimum_gain  maximum_gain  full_test_gain
   efficientnet_b0 equal_camera       0.02451       0.03928         0.03563
   efficientnet_b0        image       0.00328       0.01313         0.00541
mobilenet_v3_large equal_camera       0.01938       0.04160         0.03315
mobilenet_v3_large        image       0.00756       0.02129         0.01110
```
```text
   weighting  cameras  full_test_difference  minimum_omitted_camera_difference  maximum_omitted_camera_difference  sign_flips  direction_changes sign_flip_camera_ids
equal_camera       28               0.00248                           -0.00761                            0.00725           2                  2         ["69", "73"]
       image       28              -0.00570                           -0.00816                           -0.00274           0                  0                   []
```
Leave-one-camera-out calculations keep predictions and policies fixed. They show whether omitting
a location changes each model's NLL gain or reverses the comparison between architectures.
A sign flip is descriptive influence, not evidence of significance or a reason to exclude the camera.
The full original test-camera set remains the reported evaluation population.

## Uncertainty and limitations
All analyses inherit 2000 shared whole-camera bootstrap draws with seed
20261026 and 15 confidence bins from Notebook 04. They average the same
3 seed-specific scores inside each draw. Percentile intervals condition on fitted models,
temperatures, policies and this camera split. They exclude refitting/split uncertainty, are exploratory
across supporting outcomes, and do not establish equivalence when they cross zero. Undefined
bootstrap risks remain missing and valid/total replicate counts are reported. Equal-camera weighting
increases the influence of sparsely observed sites; ECE is bin-dependent. Detector pretraining overlap
with Caltech and different classifier pretraining recipes remain limitations. The actual Notebook 04
policy outcomes are retained above; no v1 target-failure conclusion is copied into this report.

## Output files
Representation: camera_concentration_by_split.csv, camera_event_counts.csv, class_camera_counts.csv,
category_camera_concentration.csv, class_distribution_by_split.csv. Performance: test_per_camera.csv,
weighting_comparison.csv, weighting_seed_means.csv, within_camera_diagnostics.csv. Abstention:
seed_acceptance_summary.csv, camera_abstention_summary.csv, frozen_policy_outcomes.csv. Calibration:
notebook04_gain_consistency.csv, camera_calibration_gains.csv, camera_benefit_counts.csv,
weighted_camera_bootstrap_intervals.csv, calibration_gain_contrasts.csv. Influence:
leave_one_camera_out_nll_gains.csv, leave_one_camera_out_summary.csv,
architecture_camera_influence.csv, architecture_camera_influence_summary.csv.
The parent evaluation and its cached predictions remain unchanged.
