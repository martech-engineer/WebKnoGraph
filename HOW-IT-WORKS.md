# How WebKnoGraph Works

## 1. Project Overview and Goals

WebKnoGraph is a project designed to revolutionize website internal linking by leveraging data processing techniques, vector embeddings, and graph-based link prediction algorithms. The primary goal is to create an intelligent solution that optimizes internal link structures, thereby enhancing SEO performance and user navigation. It aims to provide the first publicly available and transparent research for academic and industry purposes in end-to-end SEO and technical marketing.

The whole project is designed so that non-technical people can run the initial setup once and then work only with the notebook UIs. Each step below is a Gradio UI launched from a Colab notebook; nothing else is needed.

The methodology and the results are described in the arXiv paper: [WebKnoGraph: GNN-Powered Internal Linking](https://arxiv.org/abs/2606.06106). The paper is the only reference document for this project.

## 2. Directory Structure

```
WebKnoGraph/
├── assets/             # Project assets (images, logos)
├── data/               # Crawled data, embeddings, link graph, and model artifacts
│   ├── crawled_data_parquet/    # Raw crawl data from the crawler
│   ├── url_embeddings/          # Vector embeddings for URLs
│   ├── link_graph_edges.csv     # Extracted internal links (FROM, TO edge list)
│   ├── url_analysis_results.csv # PageRank, HITS, folder depth per URL
│   ├── crawler_state.db         # Crawl state, enables resuming
│   └── prediction_model/        # Trained GraphSAGE model and metadata
├── notebooks/          # Jupyter notebooks acting as Gradio UIs for each module
│   ├── crawler_ui.ipynb
│   ├── embeddings_ui.ipynb
│   ├── link_crawler_ui.ipynb
│   ├── pagerank_ui.ipynb
│   ├── link_prediction_ui.ipynb
│   └── automatic_link_recommendation_ui.ipynb
├── results_2026/       # Link selections and experiment outputs behind the arXiv paper
│   ├── base_file_types/         # Target URL files per strategy
│   ├── automatic_led/           # Automatically chosen link candidates and their results
│   ├── expert_led/              # Expert-chosen link candidates and their results
│   ├── FineWeb_Data_Prep.ipynb
│   ├── deltas_BA_networkit_turbo.ipynb
│   ├── deltas_Real_WWW_networkit.ipynb
│   ├── expert_deltas_BA_networkit_turbo.ipynb
│   ├── expert_deltas_Real_WWW_networkit.ipynb
│   ├── OVERALL_AVERAGES_TRACKER_*.csv
│   ├── SSA_analysis.ipynb
│   └── ssa_results_stable.csv
├── src/                # Core source code
│   ├── backend/        # Backend logic
│   │   ├── config/     # Configuration for each module
│   │   ├── data/       # Data loading, saving, state management
│   │   ├── graph/      # Graph-specific algorithms (PageRank, HITS)
│   │   ├── models/     # Machine learning model definitions (GraphSAGE)
│   │   ├── services/   # Core business logic and orchestration
│   │   └── utils/      # Utility functions (HTTP, URL processing, text extraction)
│   └── shared/         # Components shared (e.g., logging, interfaces)
├── tests/              # Unit tests
│   └── backend/
│       └── services/
├── .github/            # GitHub Actions workflows
├── LICENSE
├── README.md
├── requirements.txt
└── ... (other project files)
```

## 3. Initial Setup

1.  **Clone the project** locally from GitHub.
    *   Before you start, disable your screensaver. Some processing steps run for a long time and a screensaver can interrupt them.
2.  **Upload the project to Google Drive.**
    *   You need sufficient free storage, ideally an empty Drive.
    *   The upload takes around 15 to 20 minutes.
3.  **Mount Google Drive.** Create a notebook in the root of the `WebKnoGraph` folder in Drive, run the cell below, and grant the requested permissions:

    ```python
    # Part of the first cell in each notebook
    from google.colab import drive

    drive.mount("/content/drive", force_remount=True)
    ```

4.  **Install the requirements.** Open the terminal, go to the `WebKnoGraph` project folder, and install the dependencies from `requirements.txt`.
5.  **Run the tests** from the project root:

    ```bash
    python -m unittest discover tests/backend/services/
    ```

    A healthy setup reports 32 tests passed with `OK`.

Once the tests pass, continue with the notebooks in `notebooks/`. **Always mount Drive first, then do the rest of the setup in each notebook.**

## 4. The Workflow, Step by Step

The modules are run in sequence, because the output of one is the input of the next. The repository already contains the data collected for the Kalicube website, so every step can be explored without recollecting anything. To work on your own website, follow the same steps from scratch.

### 4.1. Web Crawling
-   **UI Notebook:** `notebooks/crawler_ui.ipynb`
-   **Service:** `src/backend/services/crawler_service.py`
-   **Configuration:** `src/backend/config/crawler_config.py`
-   **How to use it:**
    1.  Run all cells in the Colab notebook. The crawler UI is available once every cell has finished.
    2.  Already collected data is stored in the `data/` folder. To start a new crawl, empty that folder and then collect data from your target website.
    3.  A typical configuration is the **BFS** strategy, `/` as the allowed path segment, and **700 pages per session**.
    4.  Crawl statistics are shown at the end of the run.
-   **What it does:**
    -   Fetches pages (`src/backend/utils/http.py`), extracts their text (`src/backend/utils/text_processing.py`), and filters URLs by the defined rules (`src/backend/utils/url.py`).
    -   Saves the raw crawl data to `data/crawled_data_parquet/`, partitioned by crawl date.
    -   The crawler is designed with resuming in mind: crawl state is kept in `data/crawler_state.db`, so an interrupted crawl continues where it stopped.

### 4.2. Embeddings Generation
-   **UI Notebook:** `notebooks/embeddings_ui.ipynb`
-   **Service:** `src/backend/services/embeddings_service.py`
-   **Configuration:** `src/backend/config/embeddings_config.py`
-   **How to use it:**
    1.  Run all cells, as before.
    2.  The embeddings for our website are already precomputed. For a custom website (ideally 1 to 10,000 pages) you need to generate the embeddings from scratch.
-   **What it does:**
    -   Reads the crawled text from `data/crawled_data_parquet/`.
    -   Generates a vector embedding for the content of each URL (`src/backend/utils/embedding_generation.py`).
    -   Saves the embeddings to `data/url_embeddings/`.

### 4.3. Link Graph Extraction
-   **UI Notebook:** `notebooks/link_crawler_ui.ipynb`
-   **Service:** `src/backend/services/link_crawler_service.py`
-   **Configuration:** `src/backend/config/link_crawler_config.py`
-   **How to use it:**
    1.  Run all cells and use the UI to build the link profile of the website. This is already precollected for the Kalicube website.
-   **What it does:**
    -   Extracts the internal hyperlinks of each page and keeps only links within the site (`src/backend/utils/link_url.py`).
    -   Builds `data/link_graph_edges.csv`, an edge list with FROM and TO links. These edges are what make the link graph possible, and every later step depends on them.

### 4.4. PageRank and Depth Analysis: Choosing Target Pages
-   **UI Notebook:** `notebooks/pagerank_ui.ipynb`
-   **Service:** `src/backend/services/pagerank_service.py`
-   **Configuration:** `src/backend/config/pagerank_config.py`
-   **Graph Logic:** `src/backend/graph/analyzer.py`
-   **How to use it:**
    1.  Run all cells and use the UI to find pages at a specific depth level and PageRank value that are worth targeting for further optimization.
    2.  Create a file of targeted URLs by hand. This list is carried into the next step, where each URL is processed one by one.
-   **What it does:**
    -   Loads the link graph from `data/link_graph_edges.csv` and computes PageRank, HITS (hub and authority) scores, and folder depth per URL.
    -   Saves the results to `data/url_analysis_results.csv`.

### 4.5. Link Prediction with GraphSAGE: Finding Candidates
-   **UI Notebook:** `notebooks/link_prediction_ui.ipynb`
-   **Services:**
    -   `src/backend/services/graph_training_service.py` (model training)
    -   `src/backend/services/recommendation_engine.py` (recommendations)
-   **Configuration:** `src/backend/config/link_prediction_config.py`
-   **Model Definition:** `src/backend/models/graph_models.py`
-   **How to use it:**
    1.  Go through the targeted URLs from step 4.4 link by link.
    2.  For each target, set your criteria and let GraphSAGE propose the pages that could link to it and boost it. For example, if the target page is at level 5, you can limit the boosting pages to level 2 or higher in the hierarchy.
    3.  From the proposed candidates, choose the ones that make sense based on SEO expertise and template compatibility. GraphSAGE recommends; the expert makes the final choice.
-   **What it does:**
    -   Builds a graph where nodes are URLs, node features are their embeddings (`data/url_embeddings/`), and edges are the internal links (`data/link_graph_edges.csv`).
    -   Trains a GraphSAGE model to predict links and saves the model and its artifacts to `data/prediction_model/`.
    -   For a given URL, ranks pages that are not yet linked to it by their predicted score.

### 4.6. Automatic vs. Expert-Led Selection
There are two ways to turn GraphSAGE recommendations into a final set of links:

-   **Expert-led:** candidates are picked manually in `notebooks/link_prediction_ui.ipynb`, as described in step 4.5.
-   **Automatic:** candidates are picked fully automatically with `notebooks/automatic_link_recommendation_ui.ipynb`.

In both cases the link options are organized into several strategies: **folder**, **high**, **low**, **mixed**, and **random** candidates (plus **high boosters** for the automatic selection).

## 5. From Link Selections to the Paper Results

The selected links are stored in `results_2026/`, which holds everything needed to reproduce the experiments:

-   **Link selections:** `base_file_types/` holds the target URL files per strategy, and `automatic_led/` and `expert_led/` hold the chosen candidates in `*_batches/` folders, one per strategy.
-   **Network construction:** the selections are evaluated on the FineWeb (Real WWW) network and on synthetic Barabási-Albert (BA) networks, for both the automatic and the expert-led datasets. `FineWeb_Data_Prep.ipynb` prepares the FineWeb data.
-   **Deltas notebooks:** each combination has its own dedicated notebook:

    | Selection | BA network | Real WWW (FineWeb) network |
    |-----------|------------|----------------------------|
    | Automatic | `deltas_BA_networkit_turbo.ipynb` | `deltas_Real_WWW_networkit.ipynb` |
    | Expert-led | `expert_deltas_BA_networkit_turbo.ipynb` | `expert_deltas_Real_WWW_networkit.ipynb` |

-   **Overall averages:** the resulting graph statistics are collected in the `OVERALL_AVERAGES_TRACKER_*.csv` files (`AUTOMATIC_BA`, `AUTOMATIC_WWW`, `EXPERT_BA`, `EXPERT_WWW`).
-   **SSA:** `SSA_analysis.ipynb` consumes part of these tracker files to calculate SSA, with the output in `ssa_results_stable.csv`.

This is how we get to the end results shown in the [arXiv paper](https://arxiv.org/abs/2606.06106).

## 6. Key Technologies and Libraries Used

-   **Python:** Core programming language.
-   **Google Colab & Google Drive:** Runtime and persistent storage.
-   **Jupyter Notebooks & Gradio:** Interactive UIs for each module.
-   **Pandas:** Data manipulation and analysis.
-   **Parquet (pyarrow):** Storage of crawled data and embeddings.
-   **SQLite (sqlite3):** Crawler state management.
-   **Sentence Transformers (Hugging Face):** Text embeddings.
-   **PyTorch & PyTorch Geometric (PyG):** GraphSAGE model for link prediction.
-   **NetworkX:** PageRank and HITS analysis on the link graph.
-   **NetworKit:** Graph statistics in the `results_2026/` deltas notebooks.

## 7. Starting a Fresh Crawl

To begin analysis for a new website or to restart, empty the `data/` folder. This ensures no residual data from previous runs interferes with the new session. Then repeat the workflow from step 4.1.

For setup details and troubleshooting, refer to the main `README.md`. For the methodology and results, refer to the [arXiv paper](https://arxiv.org/abs/2606.06106).
