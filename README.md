# Explainable and leakage-controlled deep learning for citrus disease identification

Code, image lists, fold assignments and result tables for two studies from the MS thesis
*"An Explainable Deep Learning Framework for Early-Stage Automated Identification of Citrus Diseases in Orange Orchards"*
(Aima Irshad, Department of Artificial Intelligence, Superior University, Lahore, Pakistan and Dr. Ahmad Mustafa IIUI):

| | Paper | Status |
|---|---|---|
| **Paper 1** | *Near-Duplicate Image Leakage and Dataset Shortcuts Inflate the Accuracy of Deep Learning Models for Citrus Disease Identification* | submitted to *Computers, Materials & Continua* |
| **Paper 2** | *An Explainable Deep Learning Framework for Citrus Disease Identification: Quantitative Grad-CAM Evaluation, Background Randomisation and External Validation* | in preparation for *Computers and Electronics in Agriculture* |

**No images are stored in this repository.** All three image datasets are public and must be downloaded from
their original sources (see [DATA.md](DATA.md)); `scripts/prepare_data.py` rebuilds the exact pooled training
collection from those downloads using the SHA-256 checksums in `data/manifest_training_images.csv`.

## Repository structure

```
notebooks/   the four Kaggle notebooks that produced every number in both papers (run in this order)
scripts/     the same notebooks exported as Python scripts, plus prepare_data.py
data/        image manifests (with checksums), near-duplicate groups, fold assignments, object-mask coverage
results/     result tables (CSV) written by each notebook
```

| Notebook | What it does | Used in |
|---|---|---|
| `01_benchmark_duplicate_audit_grouped_cv_gradcam` | pools the 1,834 images; near-duplicate audit (exact hash, rotation/flip-invariant pHash, embedding cosine similarity); grouped stratified 5-fold CV of seven CNNs; McNemar tests; leakage simulation; shortcut probes (corner patches, source identification); cross-dataset test; quantitative Grad-CAM tests of ResNet50 | Paper 1 (all main results); Paper 2 (Section 4.1; Tables 2–3) |
| `02_background_randomisation_transfer_external` | object masks; background-randomised training of ResNet50 and MobileNetV2 on the same folds (A1); cross-dataset transfer with 3 seeds (A2); external test on the sweet-orange leaves of Emon et al. with 3 seeds (B) | Paper 1 (Table 10, transfer with three seeds); Paper 2 (Sections 4.4–4.6, Table 5) |
| `03_paper1_random_split_threshold_sensitivity_latency` | random (ungrouped) vs grouped 5-fold CV; near-duplicate threshold sensitivity (cosine 0.90 and 0.97); CPU latency of the seven architectures | Paper 1 (Section 3.9; Table 5, latency column; Table 11) |
| `04_paper2_paired_gradcam_efficientnet_b0` | paired Grad-CAM comparison of baseline and background-randomised ResNet50 on the same folds; background randomisation for EfficientNet-B0 (CV, transfer, external test) | Paper 2 (Section 4.3, Table 4; EfficientNet-B0 columns of Table 5) |

Cells 12–13 of notebook 01 prepare the expert rating sheet used in a separate thesis chapter and are not needed for either paper.

## How to reproduce

The notebooks were run on Kaggle Notebooks (GPU T4; TensorFlow 2.21.0, Keras 3.13.2). Each notebook finds its
inputs automatically under `/kaggle/input`.

1. Download the three datasets (links in [DATA.md](DATA.md)) and extract them.
2. Rebuild the pooled training collection:
   ```
   python scripts/prepare_data.py --rauf <Rauf download> --orange <Orange Diseases download> --out split_dataset
   ```
   The script reports how many images matched by checksum (exact copies of the files used in the papers).
3. Zip `split_dataset/` and upload it to Kaggle as a (private) dataset. Upload the extracted Emon et al.
   download as a second dataset.
4. Notebook 01: create a Kaggle notebook, import `notebooks/01_...ipynb`, add the `split_dataset` dataset via
   *Add Input*, enable the GPU and run all cells. Its output folder is `citrus_v2_results`.
5. Notebooks 02, 03 and 04: add the `split_dataset` dataset, the Emon et al. dataset (02 and 04) and the output of
   notebook 01 (it provides `images_groups_folds.csv` and the baseline out-of-fold predictions) via *Add Input*,
   then run all cells. The notebooks are resumable: if a session stops, add the partial output as an input and
   run again.

Seeds: 42 for cross-validation; 0, 1, 2 for the transfer and external tests. GPU training is not bit-wise
deterministic: repeating a run can change fold accuracies by several percentage points (Paper 2 reports two
runs of the ResNet50 comparison for this reason).

The saliency-area severity pilot mentioned in Paper 2 (Section 4.2) used a model from an earlier single-split
experiment and is not part of these notebooks.

## Key data files

* `data/manifest_training_images.csv`: one row per training image — file name used by the notebooks, class,
  source dataset, organ, near-duplicate group, fold of the grouped 5-fold CV, SHA-256 checksum and original
  path within the source download.
* `data/manifest_external_images.csv`: the 1,389 external test images (Citrus canker 588, Citrus greening 254,
  Healthy leaf 547; "Original Image" folder of Emon et al.).
* `data/alt_groupings_folds.csv`, `data/random_folds.csv`: groupings and folds of the sensitivity analyses
  (Paper 1).
* `data/mask_coverage.csv`: whether a usable automatic object mask was obtained for each training image.

## Citation

If you use this code or the fold assignments, please cite the papers (citations will be added on publication)
and the three datasets listed in [DATA.md](DATA.md). See also [CITATION.cff](CITATION.cff).

## Licence

Code and the files in `data/` and `results/`: MIT licence (see [LICENSE](LICENSE)). The images are not covered by
this licence; they remain under the licences of their original datasets.
