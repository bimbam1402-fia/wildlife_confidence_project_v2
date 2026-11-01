# Wildlife v2: event-first camera allocation and controlled sampling
Generated: 2026-10-31T16:30:36.913277+00:00

## Scope, research validity and class selection
Five known wildlife categories: ['coyote', 'deer', 'fox', 'rabbit', 'skunk']; class-index mapping: {'coyote': 0, 'deer': 1, 'fox': 2, 'rabbit': 3, 'skunk': 4}. These were pinned **before splitting** in Notebook 01 v2, for continuity with the earlier study, not chosen as the most frequent. All-category event counts and camera diversity are in `data/prepared/category_review.csv`.
The task is closed-set and conditional on the reference category being one of the five. It does not measure detection of empty scenes, unknown animals or end-to-end automation of an unfiltered camera stream. Test-set classifier performance was not consulted for v2 allocation, categories, image selection or cropping.

## Clean events before camera allocation
Notebook 01 v2 removed ambiguous images/sequences, then picked one reproducible frame per event globally. There were 47 excluded ambiguous sequence IDs. No event is duplicated or split between cameras. Only then did the optimizer allocate cameras, using metadata counts and category support. This is a custom CCT design, not the CCT20 published benchmark protocol, and it is **not** a random sample of deployment cameras.

### Development eligible event support
| class_name | train | validation | calibration | policy |
| --- | --- | --- | --- | --- |
| coyote | 5758 | 866 | 1013 | 915 |
| deer | 3630 | 587 | 642 | 656 |
| fox | 467 | 84 | 89 | 82 |
| rabbit | 3207 | 521 | 575 | 580 |
| skunk | 503 | 91 | 89 | 99 |

### Development cameras per category
| class_name | train | validation | calibration | policy |
| --- | --- | --- | --- | --- |
| coyote | 46 | 11 | 9 | 12 |
| deer | 28 | 5 | 6 | 7 |
| fox | 30 | 3 | 8 | 5 |
| rabbit | 32 | 10 | 8 | 5 |
| skunk | 37 | 7 | 6 | 9 |

## Camera-disjoint v2 samples
Training uses a cap of 1200 events per category and a predeclared 30% per-camera class budget. Validation/calibration/policy select up to 1500 events each, permitting at most 20% of the intended sample from any single camera (at most 300 per camera). Evaluation cameras are different from training and each other. Test sampling is uniform by eligible event with budget 3000, and **is not capped by camera**, preserving its selected event mixture. The fitted temperature/policy therefore represent camera-capped development mixtures; they are not automatically valid for natural site frequency mixtures.

### Selected development category counts
| class_name | train | validation | calibration | policy |
| --- | --- | --- | --- | --- |
| coyote | 1200 | 597 | 653 | 611 |
| deer | 1200 | 406 | 421 | 397 |
| fox | 467 | 65 | 58 | 58 |
| rabbit | 1200 | 372 | 316 | 373 |
| skunk | 503 | 60 | 52 | 61 |

### Largest camera shares *after sampling*
split
calibration    0.2000
policy         0.2000
test           0.2987
train          0.1910
validation     0.2000

### Data flow (test values are sampling bookkeeping, not outcome evaluation)
| split | eligible_events_after_notebook01 | selected_events_after_camera_sampling | verified_image_files |
| --- | --- | --- | --- |
| train | 13565 | 4570 | 4570 |
| validation | 2149 | 1500 | 1500 |
| calibration | 2408 | 1500 | 1500 |
| policy | 2332 | 1500 | 1500 |
| test | 5924 | 3000 | 3000 |

## Detector, crops and sample integrity
Official LILA precomputed raw MegaDetector v5a.0.0 output: https://lila.science/public/lila-md-results/caltech-camera-traps_mdv5a.0.0_results.json.zip; archive SHA-256: 6caba6a78795a24320e83697fab40cd7dd527a245ae1810611ca704696cf20b6. No local detector inference or repeat-detection elimination. Highest-confidence animal box >= 0.20, expanded 5% per side and clipped to the full original resolution; then aspect-preserving resize/pad to 224x224 RGB. No qualifying box receives a **full-frame fallback**; technical errors block technical readiness. There are 11323 planned crops, 747 full-frame fallbacks and 756 images with multiple qualifying animal boxes. Crops and image-level category agreement are NOT automatically verified by the detector or file hash.
Use `quality_review_template.csv` and `quality_review_contact_sheet_*.png` to manually inspect **training only** examples, especially fallbacks and multi-box cases. Manual review completed=False; rows explicitly marked with a failure=0. This is **separate** from technical readiness; unreviewed samples may have crop-label mismatch. Do not review test samples to tune crop rules.

MegaDetector's historical training sources include Caltech Camera Traps; unseen-camera claims apply to downstream classifier training/fitting, not necessarily the pretrained detector. Compared torchvision backbones use different pretrained ImageNet weights. Any change to crop protocol, category set, camera assignment, or sampling algorithm must be a separately declared experiment tested on new untouched data.

## Run status and provenance
Processed scope: all; verified 12070 of 12070 inputs; **technical** readiness for training=True; cross-split exact duplicate rows=0. These exact hashes do not guarantee absence of near-duplicates across different cameras. Pilot status=not_run; it is a speed/integrity check, not final model evaluation.
Selected sample SHA-256: 354af48bc70f501a89490b28f961fe3c20b8a81967ebf966b4a785333cee8676
Input protocol SHA-256: c2dd4abab6d3ffa1bad287dc84d808ab6b54c36dc5ed1d79b4dc8ac86259966b
Metadata SHA-256: 976bd9bc9380df7a03018ac70b5340bd2ff81d79a99271058f026a539450d1d6
Versions: Python 3.11.16, NumPy 2.4.6, Pandas 3.0.5, Pillow 12.2.0.

## References
Dussert et al. (2024), *Being confident in confidence scores: calibration in deep learning models for camera trap image sequences*, https://doi.org/10.1002/rse2.412
Beery, Van Horn & Perona (2018), *Recognition in Terra Incognita*, https://beerys.github.io/CaltechCameraTraps/
Precomputed MDv5a outputs: https://lila.science/megadetector-results-for-camera-trap-datasets/
Guo et al. (2017), *On Calibration of Modern Neural Networks*, https://proceedings.mlr.press/v70/guo17a.html
