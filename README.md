# Explicit–implicit gap in false-dilemma detection: a pre-registered multi-model study

Pre-registered test of how well three language models detect the *false dilemma* fallacy in news comments when the usual lexical marker ("either … or", "only two options") is absent, using the public CoCoLoFa corpus. Part of the defensive layer of the Politomorphism Engine (see [politomorphism-defense](https://github.com/politomorphism/politomorphism-defense)). The code only classifies existing text; it generates nothing.

Author: Șerban, G. F. · ORCID [0009-0000-2266-3356](https://orcid.org/0009-0000-2266-3356)

## Design in one paragraph

440 comments from CoCoLoFa (Yeh, Wan & Huang, EMNLP 2024) are split into four cells by gold label (false dilemma or not) and marker (present or absent): A easy-positive, B **hard-positive** (false dilemma without marker), C **hard-negative** (marker without false dilemma), D easy-negative. The primary outcome is writer-equal balanced accuracy on the hard cells B and C. The decision rule was fixed in advance: SUPPORTED if the estimate is at least 0.75 and the lower confidence bound is above 0.60; NOT SUPPORTED if it is at most 0.60 and the upper bound is below 0.75; otherwise INCONCLUSIVE. Models: gemini-3.1-flash-lite (M1), gemini-3.5-flash-lite (M2), gemma-4-31b-it (M3); gpt-4o-mini (M0) is a reference without a verdict.

## Result of the primary analysis (run 1)

| Model | Balanced accuracy, hard cells | 95% interval | Verdict |
|---|---|---|---|
| M1 gemini-3.1-flash-lite | 0.724 | 0.629–0.817 | INCONCLUSIVE |
| M2 gemini-3.5-flash-lite | 0.681 | 0.589–0.776 | INCONCLUSIVE |
| M3 gemma-4-31b-it | 0.621 | 0.520–0.721 | INCONCLUSIVE |
| M0 gpt-4o-mini (context only) | 0.556 | 0.464–0.652 | no verdict |

All three pre-registered verdicts are INCONCLUSIVE: the data neither support nor reject the hypothesis. The full analysis is in `benchmark/mm_report.txt`, including the detection gap, the false-alarm gap, paired contrasts, writer structure and abstentions. One writer contributes 53 of the 110 hard-positive comments, which is why the primary outcome weights writers equally. M3 returned failed calls in all three runs (10, 40 and 45 of 440 calls), so its numbers are the least stable. Predictions of runs 2 and 3 are included; the two results papers in `paper/` discuss them.

## What is in this repository

- `prereg/` — pre-registration v10 (frozen 18 November 2026, .docx and .pdf), `FREEZE_MANIFEST.txt` with the SHA-256 of every frozen file, provider documentation and rate limits checked at freezing.
- `benchmark/` — sampling script (28), prompt and parser (23), marker expression (26), launcher (33), frozen analysis (34), design analysis (31, 32), annotation manifest script (35); the sample manifest, per-run predictions `mm_pred_M{1,2,3}_run{1,2,3}.csv`, progress and usage logs, and `mm_report.txt`.
- `paper/` — results of the three-model study, English and Romanian (.docx).

## Verifying the freeze

The freeze commit of the original working repository is not part of this history, which starts at publication. The files are unchanged: compute SHA-256 of each file listed in `prereg/FREEZE_MANIFEST.txt` and compare. A mismatch means the file differs from the one frozen on 18 November 2026.

## Data: not re-hosted

No comment text is in this repository. The manifest and predictions contain only comment ids, split, cell, gold label, source label, marker flag, writer id and model output labels. Raw model answers, which may quote comments, are excluded. To reproduce, clone CoCoLoFa at the commit pinned in `benchmark/28_cocolofa_sample.py` (Crowd-AI-Lab/cocolofa, c39d45fdd57401e1f6cb674f25113dfa0304e734); the script refuses any other file.

## Reproduce

```
python -m pip install -r requirements.txt
python benchmark/28_cocolofa_sample.py <path to cocolofa clone>
set GEMINI_API_KEY=...                       (never commit a key)
python benchmark/33_cocolofa_multimodel_run.py --check
python benchmark/33_cocolofa_multimodel_run.py --run 1     (then --run 2, --run 3)
python benchmark/34_cocolofa_multimodel_analysis.py
```

Scripts expect to run from the repository root. Model identifiers and provider behaviour change over time, so a rerun may not match the logged predictions.

## Limits

Zero-shot prompts only; one corpus; hard-cell samples of 110 comments from a small number of writers; the human annotation stage in `mm_annotation_manifest.csv` has not been run. No claim of validation beyond the pre-registered verdicts above.

## How to cite

Șerban, G. F. (2026). *Explicit–implicit gap in false-dilemma detection across three language models: pre-registration and results.* GitHub: politomorphism/politomorphism-multimodel.

## License

MIT for the code (see `LICENSE`). The documents in `prereg/` and `paper/` are © the author. CoCoLoFa has its own license; check it before reusing the corpus.
