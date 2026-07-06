# Instagram Usage and User Well-Being: Segmentation of 200,000 Users

**Rush 4, Data Pro Max Bootcamp (Epitech, MSc MSI 2028)**
**Author:** Gendell Janssens · July 2026

Instagram's Innovation Department wants two things: a measure of how platform usage relates to user well-being (stress, sleep, happiness), and user profiles built from actual usage patterns. This repository contains the full study: analysis notebook, client report, technical report and defense deck.

Live Colab workbook: https://colab.research.google.com/drive/1G8AKoqsw-zFK5mJtFfE1DkIknY26cgoz

---

## Key results

| Persona | Share | Usage/day | Stress (0-40) | Happiness (1-10) |
|---|---|---|---|---|
| The Peaceful Minimalist | 6% | 5 min | 4.1 | 8.1 |
| The Casual Browser | 31% | 1h26 | 10.1 | 6.2 |
| The Engaged Regular | 31% | 3h17 | 21.2 | 5.5 |
| The Intensive User | 31% | 5h19 | 31.8 | 4.3 |

- **+5.4 stress points per extra daily hour** of usage (r = 0.83, linear across the whole range). This is an association, not a causal effect.
- **Dose over style:** usage volume carries about 89% of the predictive signal for stress. Content mix (Reels, Feed, Messages) barely matters.
- **Happiness collapses at the top end:** the decline accelerates past 5h/day, which concentrates the case for intervening on heavy users.
- **Sleep is unrelated to usage** in this dataset (r = 0.00). A null result, reported as such.
- **Minors (13-17) skew toward intensive usage:** they make up 12.6% of Intensive users vs 3.9% of Minimal users. Under the EU DSA, that is a number regulators ask about.
- **The dataset is provably synthetic** (stress and happiness are mutually independent, lifestyle carries zero signal, zero missing values in 200k rows). Effect sizes describe the generator; the methodology is what transfers to production data.

## Method in one paragraph

The two provided CSVs were compared line by line (identical on all behavioral and well-being columns, so one was kept). Ten behavioral features were engineered: session depth, scroll speed, content mix, interaction style. K-Means clustering, validated by four independent criteria (silhouette 0.55 at k=2, Calinski-Harabasz, Davies-Bouldin, elbow), finds exactly two natural behavioral groups: 94% active, 6% minimal. Business granularity is then added through explicit usage-intensity tertiles within the active group, which yields the 4 personas. Well-being variables were excluded from clustering to avoid circularity, and overlaid afterwards. A prediction track (linear baseline vs Random Forest, 80/20 split) quantifies the usage-stress association and formally tests lifestyle confounders: R² = 0.000, none exist in this data.

## Repository structure

```
├── notebooks/
│   └── Instagram_Wellbeing_Segmentation_v2.ipynb    # full analysis, runs top to bottom
├── reports/
│   ├── Rapport_Client_Instagram_BienEtre_v2.docx    # business report (French)
│   ├── Rapport_Technique_Instagram_BienEtre_v2.docx # methodology report (French)
│   └── Presentation_Client_Instagram_BienEtre_v2.pptx # 10-slide defense deck + speaker notes
├── figures/                                          # all charts (PNG)
├── data/                                             # place the two CSVs here (not tracked)
└── requirements.txt
```

## Reproducing the analysis

1. Download the dataset from Kaggle: [Social Media User Analysis](https://www.kaggle.com/datasets/rockyt07/social-media-user-analysis). You need `instagram_usage_lifestyleSafe.csv` and `instagram_users_lifestyleSafe.csv` (about 55 MB each; they are excluded from the repo).
2. Put both CSVs in `data/`, or next to the notebook. In Colab, the notebook mounts Google Drive by itself and looks in the project's Drive folder.
3. `pip install -r requirements.txt`
4. Run `notebooks/Instagram_Wellbeing_Segmentation_v2.ipynb` top to bottom (about 5 minutes). Seeds are fixed (`random_state=42`), so two runs give the same numbers.

## Limitations

Cross-sectional data, so no causal claims. Self-reported well-being. A synthetic generator with planted relationships. Tier boundaries are business conventions (tertiles), not natural clusters. Every rejected approach (k=4 direct clustering, DBSCAN, clustering on well-being, sleep prediction) is documented in the notebook and the technical report, with the reason it was dropped.
