# Datasets

Three public datasets were used. None of their images are redistributed here; download them from the links below
and cite the original authors.

| Dataset | Role | Images used | Download | Licence |
|---|---|---|---|---|
| **Citrus Fruits and Leaves Dataset** — H. T. Rauf et al., *Data in Brief* 26 (2019) 104340, doi:10.1016/j.dib.2019.104340 (Sargodha, Pakistan) | training and grouped cross-validation; transfer source and target | 744 of 759: 609 leaves (Black Spot, Canker, Greening, Healthy, Melanose) and 135 fruits (Black Spot, Canker, Greening, Healthy); the 15 fruit images of the Scab class were excluded because Scab is not in the five-class taxonomy | Mendeley Data, doi:[10.17632/3f83gxmv57.2](https://doi.org/10.17632/3f83gxmv57.2) | see the dataset page |
| **Orange Diseases Dataset** — J. Cristovão, Kaggle, 2024 | training and grouped cross-validation; transfer source and target | 1,090 fruit images: blackspot 206, canker 201, grenning (greening) 369, fresh (healthy) 314 | [kaggle.com/datasets/jonathansilva2020/orange-diseases-dataset](https://www.kaggle.com/datasets/jonathansilva2020/orange-diseases-dataset) | CC BY-NC-SA 4.0 |
| **Sweet orange leaf dataset** — Y. R. Emon, M. T. Ahad and G. Rabbany, *Data in Brief* 55 (2024) 110713, doi:10.1016/j.dib.2024.110713 (Kushtia, Bangladesh) | external test only (never used for training, model selection or early stopping) | 1,389 of 5,813: folder "Original Image", classes Citrus canker 588, Citrus greening 254, Healthy leaf 547 | Mendeley Data, doi:[10.17632/f7cr74mwpj.1](https://doi.org/10.17632/f7cr74mwpj.1) | see the dataset page |

Pooled training collection: 1,834 images in five classes — Healthy 394, Canker 442, Greening 589,
Black Spot 396, Melanose 13 (see `data/manifest_training_images.csv`).

Class mapping: Rauf "Blackspot" and Orange Diseases "blackspot" → Black_Spot; "Canker"/"canker" → Canker;
"Greening"/"grenning" → Greening; "healthy"/"fresh" → Healthy; "Melanose" → Melanose.

Not used: the Kaggle dataset "Citrus Leaf Disease Image", whose leaf images are byte-identical to the Rauf et al.
leaves, and the Yellow dragon class of Emon et al., which the dataset lists separately from Citrus greening and
which could not be mapped unambiguously.

## Rebuilding the training collection

The notebooks expect `split_dataset/<old_split>/<class>/<file>`; `old_split` is the train/val/test split of an
earlier single-split experiment and is ignored by the cross-validation (all 1,834 images are pooled).
`scripts/prepare_data.py` creates this layout from the original downloads; see the README.
