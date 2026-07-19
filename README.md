# Vector database benchmarking: Chroma and Qdrant evaluation

This repository contains public research artifacts for studying vector database query latency on recommender-system-style datasets. The notebooks generate or prepare vector datasets, populate vector databases, and record nearest-neighbor query latency measurements for Chroma and Qdrant.

## Repository contents

| File | Description |
| --- | --- |
| `prepare_datasets.ipynb` | Prepares the recommender-system dataset from MovieLens-style ratings data. The notebook reads raw ratings data and creates a pivoted matrix representation for later vector database experiments. |
| `generate_data.ipynb` | Generates synthetic vector datasets used for the experiments. It creates dense news-like vectors and recommender-system-like vectors with configurable dimensionality and sparsity. It also generates test query vectors. |
| `create_db.ipynb` | Populates and validates vector database collections. The notebook contains workflows for Chroma and Qdrant, including collection creation, batched inserts, and basic checks of stored vectors. |
| `estimation.ipynb` | Runs latency estimation experiments against the populated vector databases. It includes Chroma and Qdrant query benchmarks for different nearest-neighbor settings, including 100-NN and 2500-NN runs. |
| `test.ipynb` | Environment and installation scratch notebook. It records Python/package versions and installation outputs used to reconstruct the macOS environment. |
| `requirements.txt` | Minimal macOS Python dependencies needed by the notebooks, pinned from notebook `pip list` output. |
| `results.xlsx` | The experiment results file containing recorded query latency times. |

## Expected data layout

The notebooks reference the following data directories:

| Path | Purpose |
| --- | --- |
| `data/raw/` | Raw source datasets, including MovieLens ratings files such as `ratings.csv` or `ratings.dat`. |
| `data/gold/` | Prepared and generated benchmark datasets. |
| `data/testing/` | Test query vectors for latency experiments. |
| `data/testing_2500nn/` | Test query vectors used for 2500-nearest-neighbor latency experiments. |
| `data/chroma/` | Local Chroma persistent database files. |
| `data/qdrant/` | Local Qdrant storage files, including separate client directories used by some experiments. |

Large generated datasets and database storage directories may be omitted from the repository and regenerated with the notebooks.

## Dataset naming convention

Generated dataset filenames encode the dataset type, number of samples, dimensionality, and sparsity:

```text
gen_<type>_s<samples>_d<dimensions>_sp<sparsity>.csv
test_<type>_s<samples>_d<dimensions>_sp<sparsity>.csv
```

Examples:

```text
gen_ml_s69878_d10677_sp70.csv
gen_news_s69878_d5000_sp0.csv
test_ml_s600_d384_sp0.csv
test_news_s600_d10677_sp0.csv
```

Where:

- `ml` denotes collaborative-filtering-like vectors.
- `news` denotes dense news-embedding-like vectors.
- `s69878` means 69,878 vectors.
- `d10677` means 10,677 dimensions.
- `sp70` means 70 percent sparsity.
- `sp0` means dense or no sparsity applied.

## Environment

The notebook output records the experiment environment as:

```text
Python 3.13.11
```

The minimal required third-party packages for the macOS environment are pinned in `requirements.txt`:

```text
chromadb==1.4.0
numpy==2.4.0
pandas==2.3.3
qdrant-client==1.16.2
```

Install them with:

```bash
python -m pip install -r requirements.txt
```

## Workflow

1. Prepare raw recommender-system data with `prepare_datasets.ipynb`.
2. Generate benchmark datasets and query vectors with `generate_data.ipynb`.
3. Populate Chroma and Qdrant collections with `create_db.ipynb`.
4. Run latency experiments with `estimation.ipynb`.
5. Store raw recorded latency results in `results.xlsx`.

## Notes

- `os` and `time` are standard-library modules and are not included in `requirements.txt`.
- Some notebook outputs show Windows package versions from a separate environment (that was used for preliminary testing). The committed `requirements.txt` uses the macOS versions.
