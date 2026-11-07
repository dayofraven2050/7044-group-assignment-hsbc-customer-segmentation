# MSMK7044 Group Assignment — HSBC Customer Segmentation

Python analysis for MSMK7044 Assignment 1, covering data cleaning, descriptive statistics, systematic data problems, K-means, K-prototypes and hierarchical clustering.

**Research question:** Which existing HSBC customers aged 18–34 should be prioritised for a digital-channel credit-card promotion trial that generates new cards and sustained use?

## Repository contents

```text
.
├── assignment1_hsbc.ipynb                 # Analysis notebook with saved outputs
├── Assignment 1 data file - HSBC .xlsx    # Original assignment dataset
├── requirements.txt                      # Python dependencies
└── outputs/
    ├── tables/
    │   ├── Q1_technical_audit.xlsx
    │   ├── Q2_descriptive_statistics.xlsx
    │   └── HSBC_Q3_cleaned.xlsx
    └── figures/                          # Exported PNG and PDF charts
```

The original workbook contains a `Synthetic Data` worksheet. The cleaned workbook contains `Analysis Data` and `Q3 Excluded` worksheets. `Source_Row` retains the original Excel row number and is excluded from clustering and duplicate comparisons.

## Run the notebook

The saved environment reports Python **3.11.8**. From the repository root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyterlab
```

Open `assignment1_hsbc.ipynb` and run its cells in order, starting with **0 Settings**. Run Jupyter from the repository root: file paths use `Path.cwd()`. No original author's absolute path is required.

The notebook includes saved tables, plots and explanations, so its existing results can also be reviewed without rerunning the models. Running export cells overwrites corresponding files under `outputs/`.

Q5 uses four parallel workers. Change `Q5_DOC_N_JOBS` if needed for the available computer. The same code cell defines its own 50-start, 200-iteration settings. Q6 includes more than one complete-tree fitting cell developed during the analysis; running every cell will repeat that computation.

## Analysis workflow

| Part | Analysis | Main output |
|---|---|---|
| Q1 | Check missing values, parsing, finite numbers and valid categories | Technical audit and 49,997 retained records |
| Q2 | Describe age bands, monetary variables and all binary indicators | Descriptive-statistics workbook |
| Q3 | Flag negative income, zero-TRB/product combinations and exact duplicates before excluding their union | Cleaned sample of 47,882 records and exclusion log |
| Q4 | K-means with K=10 on income and TRB; exploratory K-prototypes with numerical and categorical inputs | Cluster profiles, scatter plots and PCA projection |
| Q5 | Compare K-prototypes solutions for K=2–12 using five selected features | Cost, silhouette and cluster-size comparisons; seven-segment profiles |
| Q6 | Ward hierarchical clustering with a symmetric 30-neighbour connectivity graph | Dendrogram, merge-distance jumps and seven-group interpretation |

### Final segmentation specification

The final Q5 model uses **INCOME, TRB, DIG_ACTIVE, PAYME and FX_TRANS**. AGE and the eight product indicators are used for subsequent profiling.

- Apply `ln(1+x)` to INCOME and TRB, then centre and scale by their sample standard deviations.
- Combine squared numerical distance with gamma-weighted categorical mismatches.
- Calculate gamma from average numerical variance divided by average categorical Gini impurity: approximately **2.757660** for the retained sample.
- Compare **K=2–12**, with **50 distance-weighted initialisations** and up to **200 iterations** per start.
- Evaluate silhouettes on one fixed sample of **5,000 records**.

K=7 has the highest observed mixed-dissimilarity silhouette score among the tested solutions (**0.5087**). G1–G7 are ordered by median income. Hierarchical labels H1–H7 are independently ordered and do not imply matching memberships.

## Main findings

- Raw sample: **50,000 records**.
- Q1 excludes **3 technical-error records**.
- Q3 excludes **2,115 distinct flagged records**, leaving **47,882**.
- The digital-card candidate pool (`DIG_ACTIVE = 1` and `CC = 0`) contains **18,294 records**.
- G4 and G5 contain **8,949 candidates**, representing **48.9%** of that pool, and are proposed as separate strata for an everyday-spending credit-card promotion trial.

These are segmentation findings and proposed trial decisions. The dataset does not contain results from an implemented marketing experiment. Digital-service indicators describe registration, and FX refers to the previous 12 months.

## Figures

Key exports include:

- `q4_kmeans_k10_scatter_colored`: K-means customer segments.
- `q4_kprototypes_k10_pca_vivid`: exploratory K-prototypes PCA projection.
- `q5_document_method_kprototypes_metrics`: final five-feature K comparison.
- `q5_k7_segment_sizes`: final segment sizes.
- `q5_k7_median_financial_profile`: segment median income and TRB.
- `q5_k7_customer_profile_heatmap`: digital, age and product profiles.
- `q6_hierarchical_dendrogram_colored`: full-sample constrained Ward tree.
- `q6_hierarchical_merge_distance_jump`: evidence for the seven-group cut.

The figures directory also retains earlier visual and model variants. Use the `q5_document_method_...` figures for the final Q5 specification. PCA is used for visualisation; it does not change the fitted cluster labels.

## Data and course context

The Excel file is the supplied synthetic HSBC assignment dataset. This repository records an academic exercise; the recommendations are proposals for testing, rather than HSBC policy or observed campaign outcomes.

## 中文使用说明

本仓库保存 MSMK7044 小组作业 Q1–Q6 的 notebook、原始数据、清洗数据、Excel 表格及图片。请在仓库根目录启动 Jupyter，从 Q0 开始依次运行。最终分析样本为 47,882 条；Q5 最终采用五变量 K-prototypes 七群方案，Q6 采用带 30 近邻连接约束的 Ward 层次聚类。图片目录保留了不同阶段的版本，最终 Q5 群数比较请查看 `q5_document_method_kprototypes_metrics`。
