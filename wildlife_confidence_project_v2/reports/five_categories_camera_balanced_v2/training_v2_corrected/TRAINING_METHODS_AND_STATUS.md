# Corrected v2 training: training_v2_corrected
Updated: 2026-10-31T20:54:09.136747+00:00

## Prepared data and readiness
Uses the unchanged Notebook 01/02 v2 outputs for five_categories_camera_balanced_v2. No new split, sampling,
cropping, exclusions or downloads are performed. Subset SHA-256: 354af48bc70f501a89490b28f961fe3c20b8a81967ebf966b4a785333cee8676; input
protocol: c2dd4abab6d3ffa1bad287dc84d808ab6b54c36dc5ed1d79b4dc8ac86259966b; camera split: 5d65707544cdafe9ae2208ca745623e4b1713c13a5e363ba902800963510f9fb; selection: 2efde71bb734eeafe31c29d8d8c65b3c3340216f7bd9bf564f40180b657e6614.
Selected event counts: {'train': 4570, 'validation': 1500, 'calibration': 1500, 'policy': 1500, 'test': 3000}.
Selected camera counts: {'train': 64, 'validation': 13, 'calibration': 13, 'policy': 13, 'test': 28}.
Selected category counts (metadata only, not test performance):
split       train  validation  calibration  policy  test
class_name                                              
coyote       1200         597          653     611  1307
deer         1200         406          421     397   764
fox           467          65           58      58   108
rabbit       1200         372          316     373   724
skunk         503          60           52      61    97
Manual crop verification was not performed. The optional Notebook 02 review CSV is not read
or required. Automatic checks establish file integrity and frozen-dataset consistency, not whether
the selected detector crop contains the reference category. Possible crop-label mismatches,
incorrect boxes and inadequate crops remain study limitations. The selected input population
remains five known categories, one frame per eligible event. MegaDetector uses precomputed
MDv5a predictions with documented historical Caltech training overlap; camera holdout applies
to downstream fitting, not necessarily the pretrained detector.

## Training amendment, fixed before new evaluation
This is a new training revision, not the unchanged v1 recipe. It retains the existing prepared
v2 dataset, all three seeds [42, 123, 2026], both architectures and their
specified ImageNet pretrained weights. Batch size 4; unweighted cross-entropy; horizontal
flips in training only; AdamW weight decay 0.0001. Two head epochs at 0.001, then up to ten
fine-tuning epochs at 0.0001, with a fresh optimizer. All classifier-module parameters train
during the head stage; feature parameters and feature statistics are frozen then.
During fine-tuning all parameters are trainable and BatchNorm running statistics are updated,
matching the original v2 behavior. Dropout/stochastic-depth follow the original training modes.

**Limitation — BatchNorm with small batches**

Training uses a batch size of four. During fine-tuning, BatchNorm running statistics are updated from these small batches, which may introduce instability or variability in model performance. This potential effect has not been experimentally isolated.
Early stopping: three fine-tuning epochs without a strict improvement in event-weighted
validation NLL. Final selection compares best_head.pt with best_finetune.pt on that same
metric; a cross-stage tie retains the head. Within-stage ties retain the earliest epoch.
The selected weights are copied to best_selected.pt, whose stage/epoch, candidate hashes
and source checkpoint hash are recorded. All development logits come from that exact file.
Equal-camera validation NLL is recorded each epoch as an additional diagnostic;
they do not change selection after seeing results. No calibration/policy/test performance
selects models or training settings. Test input integrity checks are not test evaluation.
The two architectures have different heads and pretraining recipes; this is not a causal
architecture-only comparison. Training drop_last=True omits a changing incomplete last batch;
validation and exports use every row without augmentation. Same prepared inputs/normalization
are used in training and downstream inference. One split and three training seeds do not
measure uncertainty across refitted datasets or new deployment cameras.

## Actual execution and provenance
Completed 6/6 runs. Pending/failed runs remain visible in
training_run_summary.csv. Backend: cpu; versions: {'python': '3.11.16', 'torch': '2.14.0+cu130', 'torchvision': '0.29.0+cu130', 'numpy': '2.4.6', 'pandas': '3.0.5', 'pillow': '12.2.0'}.
Protocol SHA-256: 71342e9f1c8029c8c17e30474306ed6a7a68fc1a125ba50f73ff4a0e5e4db0dd. Resume is at completed epoch boundaries with optimizer and
RNG state; older training_v2 runs are not resumed by training_v2_corrected. Completed runs
require all three verified development exports, matching run identity and candidate/selected
checkpoint hashes. Automatic input integrity and dataset-consistency checks do not overwrite the prepared data.
Files per run: history.csv, best_head.pt, best_finetune.pt, best_selected.pt, last_resume.pt,
completed.json, validation_logits.npz, calibration_logits.npz, policy_logits.npz. Preserve actual
weights and exports as well as their hashes. Do not claim success from the presence of a log.

## Evaluation handoff
Notebook 04 must read reports/five_categories_camera_balanced_v2/training_v2_corrected/training_index.json and load each
run's checkpoint_file (best_selected.pt); do not substitute best_finetune.pt or old v2 exports.
All six runs are retained. Temperature fitting and uncertainty-aware policy selection belong
to Notebook 04 and must precede final test inference. Calibration/policy camera concentration,
distribution shift and prior v1 test inspection remain study limitations; this training revision
does not repair them. No final test predictions are produced here.
