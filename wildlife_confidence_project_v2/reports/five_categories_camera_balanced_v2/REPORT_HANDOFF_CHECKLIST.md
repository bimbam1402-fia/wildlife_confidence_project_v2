# v2 report handoff
- Use METHODS_AND_DECISIONS.md and the actual v2 data support tables.
- Describe five categories as a frozen scope choice made before the camera split.
- Report per-camera and per-class support and maximum camera shares in the selected calibration/policy data.
- Do not equate event-weighted or capped camera samples with a typical future camera.
- Detector boxes are precomputed predictions, not true species boxes; crop/label matching requires manual review.
- Technical image readiness does not certify crop quality.
- The full five-notebook pipeline must be rerun with new v2 experiment paths, checkpoints and temperatures.
- Temperature scaling changes confidence, not class predictions; a 5% policy-set observed error is not a future error guarantee.
- The final test set must not be consulted for model selection or used to revise design.
