# Provenance and verification report

This report documents where every archived file comes from, what was changed when it was archived, what was left out, and how the content was verified. Verification date: 2026-10-27; re-checked on 2026-10-29. The split lists were re-derived on 2026-11-06. Table numbers refer to the manuscript submitted to the *Journal of Visual Communication and Image Representation*.

## 1. Source

- **Notebook:** `DWT_SVD_Watermarking.ipynb` (76 cells: 49 code, 27 Markdown). It was run from top to bottom on 2026-10-26, with execution counts 1–48 in order, no error output, and one empty final cell.
- **Code identity:** with comments ignored, the 49 code cells are identical to the earlier committed version. That version already contained the coding-layer fix of 2026-10-23 (§4). The 2026-10-26 edit only changed comments and removed two empty cells.
- **Results folder:** `results_ieee_blind_ml/` written by that run.

## 2. Runs, keys and seeds

| Run | Notebook cell | Covers | Embedding key | Attack-noise seed tag | Archived folder |
|---|---|---|---|---|---|
| Main evaluation | 36 (export: 48, 50, 52) | 100 test | per cover: `_blind_seed('ieee-test-case', 2025, i)` | `'eval-attack'` | `main_experiment/` |
| Decoder and synchronization ablation | 37 (stats: 39, 41; families: 59) | 100 test | per cover: `_blind_seed('abl-test-case', i)` | `'ablation-attack'` | `decoder_ablation/`, `statistical_analysis/` |
| Quality-oriented point R_img = 1 | 75 | 100 test | same keys as the decoder ablation | `'ablation-attack'` | `quality_operating_point/` |
| Coding-layer ablation | 70 | 100 test | single default key (`GLOBAL_SEED = 2025`) | per cover `'section11l'` | `bch_ablation/` |
| External baselines | 58 | calibration: first 10 validation; test: 100 | positions seeded by 2025 | `'extbase'` | `baselines/` |
| Synchronization tuning | 57 | first 20 validation | per cover `'synctune'` | `'synctune-attack'` | `synchronization/sync_*` |
| Off-grid rotations | 61 | first 50 test | single default key | — | `synchronization/offgrid_*` |
| Rotation then JPEG | 74 | first 20 test | single default key | `'combo'` | `synchronization/combined_*` |
| Random payloads | 63 | first 20 test | single default key | `'payload'` | `random_payloads/` |
| Key sensitivity | 55 | first 50 test | per cover `'keysens'` vs `'wrongkey'` | — | `key_sensitivity/` |
| Permutation importance | 56 | validation | — | — | `feature_importance/` |
| Payload and blindness audit | 54 | — | — | — | `payload_and_audit/` |
| Model training | 36 | 500 train (+ validation for monitoring) | — | — | not in this record |

The main evaluation, the decoder ablation and the coding-layer ablation use the same 100 test covers in the same order. They are nonetheless **independent runs** with different keys and attack realizations, so their values differ slightly (for example, attacked text BER 0.0262 vs 0.0269 with the learned decoder).

## 3. Reproducibility of the re-run

The 2026-10-26 re-run was compared with the previously committed outputs (git):

- **Byte-identical:** every result file except those listed below.
- **Changed as expected:** the three coding-layer ablation files (§4), the run-time table (machine-dependent, archived in `payload_and_audit/`) and the creation date inside `ieee_blind_ml_config.json`.
- **Model:** the retrained pickle (decompressed SHA-256 `b84840445fa2…d6dc`) differs from the pickle of the original run (written on 2026-10-09 according to its gzip header and committed to git on 2026-10-23; decompressed SHA-256 `b28a12297712…b89c`) in exactly **one byte out of 464,535,964**. That byte is the last float of the pickle, the text model's stored validation AUC, which changed from 0.9557691263584762 to 0.955769126358476 (2.2 × 10⁻¹⁶). The cause of this difference was not investigated. All trees are byte-identical. The gzip container also differs (header timestamp); the container of the re-run has SHA-256 `111ff57a…aa96`. The trained model is not part of this record.

## 4. Correction history: coding-layer ablation (Table 10)

In the original run, the context manager `_toggle_bch` did not change the global flag `BLIND_USE_BCH`, so the two "without BCH" variants were evaluated with an inconsistent configuration. The notebook was fixed on 2026-10-23 (`global BLIND_USE_BCH`), and the archived files come from the corrected re-run.

| Variant | Clean BER (before → after) | Attacked text BER (before → after) |
|---|---|---|
| Without BCH | 0.3819 → 0.0000 | 0.3644 → 0.0281 |
| Without BCH and without repetition | 0.3560 → 0.0022 | 0.4010 → 0.1204 |
| Other three variants | unchanged | unchanged |

PSNR, SSIM and NC are unchanged. The corrected values equal those of the separate errata recomputation of 2026-10-23, to floating-point precision.

## 5. Changes made when archiving

**Path substitution.** In the 4 archived files with a `cover_path` column, the byte string of the private Windows home folder of the account that ran the experiments was replaced by `<HOME>`. No other byte was changed. CRLF line endings and every value are preserved. Every non-path column was compared and is identical. Hashes before and after substitution:

| File | Replacements | SHA-256 original | SHA-256 archived |
|---|---|---|---|
| `results/main_experiment/ieee_blind_ml_summary_cases.csv` | 300 | `af4c587336a4e290f4a545be82e169ccee9bd1d232193c281ddd0dcab3f3961c` | `09be41e78f5a1045796d0ad6beb300db318422a0c054b27c606bff560d201335` |
| `results/main_experiment/ieee_blind_ml_attacks_long.csv` | 8,100 | `22d67c922ec3ead44a866eb1e8bae4731b6687edc7fef52a8e0ae67064f52784` | `ce5aea9fdc728c8510351f5b82c307869668c4686068946305dfe686020370b4` |
| `results/decoder_ablation/section14e_ablation_details.csv` | 18,900 | `44d21e1281c11218a85fe5db538cb95c331fb96b9b40b299df414d9b336b91e5` | `47ed21a00842142c66693ca85af935e606c1156fbd159e23732b9a6c59692f3b` |
| `results/bch_ablation/section11l_bch_rep_ablation_details.csv` | 13,500 | `f3ef1646265e2dcf7a3c9d1772bd160c94c00203168e3dd174f5581e4d6893b2` | `07984e304000e8489328667d56e6061a6f3516440343c654870598d0b13988f5` |

**Duplicates stored once.** The notebook writes these pairs under two names, with byte-identical content:

| Archived | Not archived (identical copy) |
|---|---|
| `section14e_ablation_details.csv` | `ieee_blind_ml_ablation_details.csv` |
| `section14e_ablation_summary.csv` | `ieee_blind_ml_ablation_summary.csv` |
| `section14e_ml_gain_stats.csv` | `ieee_blind_ml_ml_gain_stats.csv` |

## 6. Files not archived, and why

| File | Reason |
|---|---|
| `blind_extractors_ieee.pkl.gz`, `ieee_blind_ml_models.pkl` | trained models, not part of this record |
| `claims_allowed_for_article.csv`, `ieee_blind_ml_claims_allowed.csv` | hand-written notes from earlier notebook versions (e.g. 20-cover values); not results, and partly outdated |
| `ieee_blind_ml_article_checklist.csv`, `ieee_blind_ml_literature_template.csv` | templates, not results |
| `fairness_of_comparison_table.csv`, `ieee_blind_ml_fairness_table.csv` | qualitative description, not results |
| `section11k_literature_comparison_verified.csv` | literature values, not results of this study |
| `section11n_q1_executive_summary.csv` | derived summary of archived files, with rounded or outdated entries |
| `section11i_q1_threshold_check.csv` | internal development targets, not results; not cited in the manuscript |
| PNG figures | figures of the article; `qualitative_logos.png` shows third-party logos without a licence |
| chest X-ray and logo images | third-party datasets; see the README |

## 7. Split lists

`metadata/splits/` (`train_covers.txt`, `val_covers.txt`, `test_covers.txt`, `logo_files.txt`, `cover_logo_assignment.csv`) was generated on 2026-10-22 with the notebook's own split functions (pool of 700 covers, then `np.random.default_rng(2025)` permutation into 500/100/100). Paths are relative to the Kaggle dataset root. Checks:

- `test_covers.txt` equals, in order, the 100 covers recorded in the main evaluation, the decoder ablation and the coding-layer ablation;
- the three lists are pairwise disjoint (500/100/100);
- the logo of test cover *i* is `logo_files[i mod 57]`, as recorded (`watermark_id`) in the main evaluation;
- no result file records the train and validation lists; on 2026-11-06 all three lists were re-derived from the dataset file listing with the seeded permutations of the notebook, and they match in order, the test list also matching the saved results.

## 8. Independent verification

Every summary file was recomputed from its per-observation file (pandas, scipy; models not loaded):

| Experiment | Checks | Result |
|---|---|---|
| Main evaluation (Tables 3, 8) | 104 | 92 exact; 12 differ by ≤ 4 × 10⁻⁵ because the per-cover summaries are rounded (below) |
| Decoder ablation and paired statistics (Tables 5, 12), including Wilcoxon p-values and the seeded bootstrap intervals | 76 | all exact |
| Proposed rows by family (Table 13) | 56 | all exact |
| External baselines (Table 13) | 42 | all exact |
| Coding-layer ablation (Table 10) | 41 | 31 exact; 10 differ by definition (below) |
| Off-grid rotations (Table 6) | 49 | all exact |
| Rotation then JPEG (Table 7) | 25 | all exact |

Notes:

1. **Rounding of per-cover summaries.** `ieee_blind_ml_summary_cases.csv` stores each cover's mean of the 26 already-rounded per-attack values, rounded again to 4 decimals, and `ieee_blind_ml_summary_by_method.csv` averages those rounded values. Each text BER in `ieee_blind_ml_attacks_long.csv` is itself k/96 rounded to 4 decimals (notebook cell 28). Because consecutive multiples of 1/96 are more than 0.0001 apart, every integer count k is recovered uniquely as round(96 × BER); the raw bit predictions are not saved, so the counts below are a reconstruction from the saved values, not a recount. The exact means are: 6,540 reconstructed bit errors (learned decoder) and 9,832 (rule-based) out of 249,600 bits, i.e. 0.026202 and 0.039391, a relative reduction of 33.48 % (reported as 33.5 % in the manuscript). The rounded summaries give 0.026165 and 0.039428 (33.64 %), an artefact of the double rounding. The 32 % of the decoder ablation (Tables 5 and 12) comes from a separate run whose text BERs are stored at full precision (9,914 → 6,710 bit errors). At 4 decimals both give 0.0262 ± 0.0029 and 0.0394 ± 0.0033.
2. **Standard deviations in `section11l_bch_rep_ablation_summary.csv`.** The `*_attacks_std` columns are computed over all 2,600 (cover, attack) rows, not across the 100 covers as in the other summaries.
3. **Counts in `section11j_per_attack_gain_summary.csv`** (for example "15/26 attacks with NC gain > 0") use 4-decimal-rounded gains. With unrounded gains, NC improves on 19 attacks and degrades on 7. Text BER improves on 3 attacks, all with p < 10⁻⁹.
4. **QIM-DWT calibration.** The calibration used the first logo only. On test covers, QIM-DWT PSNR depends on the embedded logo (per-logo means 31.2–44.0 dB at the 35.91 dB target and 35.9–48.1 dB at the 40.64 dB target). Covers carrying the calibration logo reach the targets (35.99 and 40.64 dB), while the overall means are 33.62 and 38.29 dB. Block DWT–SVD PSNR depends much less on the logo (35.50–36.19 dB and 40.15–40.75 dB).
5. **Learned image decoder.** It changes the decoded logo in 646 of the 2,600 (cover, attack) cases, for a net gain of 196 correct bits out of 10,649,600 (BER_logo 0.109538 → 0.109519; NC_hard 0.912400 → 0.912377).
6. **Synchronization threshold.** Without margin, τ = 0.75, 0.80 and 0.85 all give zero validation error, and 25 of the 49 (margin, τ) pairs do. The run uses margin 0 and τ = 0.80.
7. **Non-blind reference size.** `section11m_payload_size_table.csv` reports 8,371,451 bytes, the value for the first test cover. Across the 100 test covers the size ranges from 8,336,582 to 8,382,588 bytes (mean 8,369,501.64).
