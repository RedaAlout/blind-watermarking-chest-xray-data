# Research data: An Integrated Blind Wavelet-Domain Watermarking Framework for Chest Radiographs with Pilot-Based Rotation Synchronization and Error-Protected Text

**Authors:** Reda Alout and Habib Ayad, Laboratory of Computer Science, Artificial Intelligence and Cybersecurity, École Normale Supérieure de l'Enseignement Technique (ENSET), Hassan II University of Casablanca, BP 159, Boulevard Hassan II, Mohammedia, Morocco.

**Purpose:** this record holds the result files and experiment records that support the article of the same title, prepared for the *Journal of Visual Communication and Image Representation*.

---

## 1. What this record contains

- the result files written by the research notebook (`results/`), byte-for-byte except for one documented path substitution (§12);
- the cover and logo split lists and the logo assignment (`metadata/splits/`);
- a manifest of every result file with its experiment, run, notebook cell and manuscript reference (`metadata/experiment_manifest.csv`);
- the software environment (`metadata/environment_information.txt`) and a provenance and verification report (`metadata/provenance_report.md`);
- `CHECKSUMS.sha256` for all files.

It contains **no medical images and no logo images** (§6). The trained models and the source code are not part of this record (§7).

## 2. Directory structure

```text
JVCIR_Watermarking_Research_Data/
├── README.md
├── CHECKSUMS.sha256
├── results/
│   ├── main_experiment/          Tables 3, 8, 9; Figs. 4, 5
│   ├── statistical_analysis/     Table 12
│   ├── decoder_ablation/         Tables 4, 5, 9, 13; Fig. 3
│   ├── quality_operating_point/  Tables 9, 13
│   ├── baselines/                Table 13; Sect. 5.6
│   ├── bch_ablation/             Tables 9, 10
│   ├── synchronization/          Tables 6, 7; Sect. 5.3
│   ├── random_payloads/          Tables 9, 11
│   ├── key_sensitivity/          Sect. 5.7
│   ├── feature_importance/       Fig. 6
│   └── payload_and_audit/        Table 14; Sect. 3.6
└── metadata/
    ├── experiment_manifest.csv
    ├── environment_information.txt
    ├── provenance_report.md
    └── splits/
```

## 3. Experiments and runs

Covers are 512 × 512 grayscale chest radiographs. The test split has 100 covers. The evaluation suite has 27 conditions: no attack plus 26 attacks, of which 6 are rotations. Several independent runs exist; **do not mix them**:

| Run | Covers | Embedding key | Stored in | Manuscript |
|---|---|---|---|---|
| Main evaluation | 100 test | one key per cover (`'ieee-test-case'`, 2025, i) | `main_experiment/` | Tables 3, 8, 9; Figs. 4, 5; abstract |
| Decoder and synchronization ablation | 100 test | one key per cover (`'abl-test-case'`, i) | `decoder_ablation/`, `statistical_analysis/` | Tables 4, 5, 9, 12, 13; Fig. 3 |
| Quality-oriented point (R_img = 1) | 100 test | same keys as the decoder ablation | `quality_operating_point/` | Tables 9, 13 |
| Coding-layer ablation (BCH, repetition) | 100 test | single key (seed 2025) | `bch_ablation/` | Tables 9, 10 |
| External baselines | calibration: 10 validation; evaluation: 100 test | fixed positions (seed 2025) | `baselines/` | Table 13; Sect. 5.6 |
| Synchronization threshold tuning | first 20 validation | one key per cover (`'synctune'`) | `synchronization/sync_*` | Sect. 5.3 |
| Off-grid rotations | first 50 test | single key | `synchronization/offgrid_*` | Table 6 |
| Rotation followed by JPEG | first 20 test | single key | `synchronization/combined_*` | Table 7 |
| Random 96-bit messages | first 20 test | single key | `random_payloads/` | Tables 9, 11 |
| Key sensitivity | first 50 test | correct vs wrong per-cover key | `key_sensitivity/` | Sect. 5.7 |
| Permutation importance | validation | — | `feature_importance/` | Fig. 6 |

`metadata/experiment_manifest.csv` gives, for each file, the notebook cell, run, partition, number of covers, conditions, metrics, manuscript reference and validation status.

## 4. Column labels

The notebook writes some labels in French:

| Label | Meaning |
|---|---|
| `Methode`, `Variante`, `Attaque`, `Famille` | method, variant, attack (condition), family |
| `Blind + ML` / `Blind sans ML` / `Non-blind actuel` | complete decoder / vote-based comparator / non-blind reference |
| `Blind + ML complet`, `image seul`, `texte seul` | both learned decoders, image decoder only, text decoder only |
| `Blind vote-hard (sans soft)` | majority vote with hard reconstruction |
| `pilotes ON` / `pilotes OFF` | pilot search enabled / disabled |
| `Sans BCH`, `Sans répétition image`, `Sans répétition texte` | without BCH, without image repetition, without text repetition |
| `NC attaques`, `BER attaques` | mean over the 26 attacks of one cover |
| `PSNR_cible` | target PSNR of a baseline calibration |
| `RDWT-SVD bloc (baseline)` | the **block DWT-SVD** baseline of the manuscript: a simplified re-implementation on a single-level decimated DWT with 4 × 4 blocks (historical label) |
| `Cropping 10%`, `Cropping 25%` | occlusion by a black square (not a geometric crop) |
| `Scaling 0.5x`, `Scaling 1.5x` | resampling round trip (output stays 512 × 512) |
| `faux positifs (non-rot)`, `echecs (rotations)`, `erreur totale` | false-positive rate, failure rate, total error rate |
| `bonne cle`, `cle fausse` | correct key, wrong key |
| `Insertion`, `Extraction`, `Entraînement`, `Modèles` | embedding, extraction, training, models |
| `Oui` / `Non` | yes / no |

## 5. Mapping between results and the manuscript

| Manuscript item | File(s) |
|---|---|
| Table 3 | `main_experiment/ieee_blind_ml_summary_by_method.csv`, `ieee_blind_ml_summary_cases.csv` |
| Table 4 | `decoder_ablation/section14e_ablation_details.csv` |
| Table 5, Fig. 3 | `decoder_ablation/section14e_ablation_summary.csv`, `section14e_ablation_details.csv` |
| Table 6 | `synchronization/offgrid_rotation_details.csv`, `offgrid_rotation_summary.csv` |
| Table 7 | `synchronization/combined_attack_details.csv`, `combined_attack_summary.csv` |
| Table 8, Figs. 4, 5 | `main_experiment/ieee_blind_ml_attacks_long.csv`, `ieee_blind_ml_attack_by_method.csv`, `section11j_per_attack_gain_table.csv` |
| Table 9 | `main_experiment/ieee_blind_ml_attacks_long.csv`, `decoder_ablation/section14e_ablation_details.csv`, `bch_ablation/section11l_bch_rep_ablation_details.csv`, `quality_operating_point/quality_point_details.csv`, `random_payloads/random_payload_details.csv` |
| Table 10 | `bch_ablation/*` |
| Table 11 | `random_payloads/*` |
| Table 12 | `statistical_analysis/section14e_ml_gain_stats.csv`, `section14e_per_image_paired_tests.csv` |
| Table 13 | `baselines/*`, `decoder_ablation/proposed_by_family.csv`, `decoder_ablation/section14e_ablation_details.csv`, `quality_operating_point/*` |
| Table 14 | `payload_and_audit/section11m_payload_size_table.csv`, `section11m_runtime_table.csv` |
| Fig. 6 | `feature_importance/feature_importance_text.csv` |
| Sect. 3.6 | `payload_and_audit/section11m_blindness_verification_table.csv` |
| Sect. 4.1 | `metadata/splits/*` |
| Sect. 5.3 | `synchronization/sync_hysteresis_tuning.csv`, `sync_angle_by_attack.csv` |
| Sect. 5.7 | `key_sensitivity/key_sensitivity.csv` |

Tables 1 and 2 contain no experimental results; Figs. 1 and 2 are diagrams of the method.

## 6. Datasets (not included)

- **Covers:** Kaggle "Chest X-Ray Images (Pneumonia)", https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia (`paultimothymooney/chest-xray-pneumonia`, version 2, 5,856 images). The data were originally deposited at Mendeley Data by Kermany *et al.* (2018), https://doi.org/10.17632/rscbjbr9sj.2, under the CC BY 4.0 licence.
- **Logos:** Kaggle "copyright-logo", https://www.kaggle.com/datasets/bibekcse/copyright-logo (`bibekcse/copyright-logo`, 57 images). The uploader specifies no licence, so the logos are not redistributed here.
- **How to obtain them:** download both datasets from Kaggle under their own terms. `metadata/splits/` lists the covers of each split and the logo assigned to each cover. Each line is the path of one file inside the downloaded Kaggle dataset, for example `chest_xray/train/PNEUMONIA/person520_bacteria_2205.jpeg` for a cover and `Copyright logo/10.png` for a logo.
- **Kaggle folders are not the splits of this study:** the 700 covers were drawn from the Kaggle `train`, `val` and `test` folders and then randomly re-partitioned (Sect. 4.1 of the article). The split of a cover is given by the file that lists it (`train_covers.txt`, `val_covers.txt` or `test_covers.txt`), not by its Kaggle folder name; for instance, `test_covers.txt` contains paths under `chest_xray/train/`.

## 7. Trained models and source code

- **Trained models:** not included in this record. They occupy 464,535,964 bytes, as stated in the manuscript.
- **Source code:** not included in this record. See the Data availability statement of the article.

## 8. Dependencies

See `metadata/environment_information.txt`. Main packages of the run: Python 3.11.9, numpy 2.4.4, scipy 1.17.1, scikit-learn 1.9.0, scikit-image 0.26.0, opencv-python 4.13.0.92, PyWavelets 1.9.0, pandas 3.0.3, matplotlib 3.10.9.

## 9. Observed reproducibility

On 2026-10-26 the corrected notebook was re-run from top to bottom. Every result file was byte-identical to the earlier run, with three exceptions: the coding-layer ablation, which was intentionally corrected (see `metadata/provenance_report.md`), the machine-dependent run-time table, and the creation date stored in `ieee_blind_ml_config.json`.

## 10. Verifying integrity

```bash
cd JVCIR_Watermarking_Research_Data
sha256sum -c CHECKSUMS.sha256        # Linux / macOS / Git Bash
```

## 11. Results verification

All summary files were recomputed independently from the per-observation files with Python and pandas. Every value used in the manuscript was reproduced. The few differences are explained in `metadata/provenance_report.md` (rounding of per-cover summaries, and a standard-deviation definition).

## 12. Modifications relative to the notebook outputs

- In the four files that contain a `cover_path` column, the private Windows home folder of the account that ran the experiments was replaced by `<HOME>`. No other byte was changed: line endings (CRLF) and all values are preserved. `metadata/provenance_report.md` gives the SHA-256 of both versions.
- Byte-identical duplicates written by the notebook under two names were stored once (listed in the provenance report).

## 13. Known limitations

- Per-cover values in `ieee_blind_ml_summary_cases.csv` are the means of the 26 already-rounded per-attack values, rounded again to 4 decimals. Each text BER in `ieee_blind_ml_attacks_long.csv` is k/96 (k erroneous bits among the 96 information bits) rounded to 4 decimals by the notebook. Consecutive multiples of 1/96 are more than 0.0001 apart, so k = round(96 × BER) is recovered uniquely for every case; the raw bit predictions are not saved, so these counts are a reconstruction from the saved values, not a recount. The exact mean attacked text BER is therefore 6,540 and 9,832 reconstructed bit errors out of 249,600, i.e. 0.026202 (complete decoder) and 0.039391 (vote-based comparator), a relative reduction of 33.48 %, reported as 33.5 % in the manuscript. Means of the rounded per-cover values give 0.026165 and 0.039428 (33.64 %), an artefact of the double rounding. The 32 % reduction quoted in the manuscript for the decoder ablation comes from a separate run (`decoder_ablation/`) whose text BERs are stored at full precision (9,914 → 6,710 bit errors).
- In `section11l_bch_rep_ablation_summary.csv`, the `*_attacks_std` columns are standard deviations over all (cover, attack) rows, not across covers.
- The QIM-DWT baseline was calibrated with one logo. Its test-set PSNR depends on the embedded logo (per-logo means from 31.2 to 44.0 dB at the 35.91 dB target and from 35.9 to 48.1 dB at the 40.64 dB target) and averages 33.62 / 38.29 dB instead of the 35.91 / 40.64 dB targets.
- The learned image decoder changes 646 of the 2,600 (cover, attack) logo decodings. Its net effect is +196 correct bits out of 10,649,600 (BER_logo 0.109538 → 0.109519).
- Run-time measurements are machine-dependent. The hardware of the run was not recorded.
- Splits were made per image, not per patient: according to the identifiers in the file names, 19 of the 100 test covers share a patient with the training or validation images.
- One modality (chest X-ray) and one dataset; no clinical evaluation.

## 14. Licence and citation

- **Licence:** the files in this record (results, metadata and documentation) are released under the Creative Commons Attribution 4.0 International licence (CC BY 4.0, https://creativecommons.org/licenses/by/4.0/). Third-party data are not included: the chest radiographs (Kermany *et al.*, 2018, CC BY 4.0) must be obtained from their source, and the logos (no licence specified by their uploader) are not redistributed.
- **Citation:** cite this record and the article.
"# blind-watermarking-chest-xray-data" 
