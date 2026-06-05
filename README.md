![WebKnoGraph](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/WebKnoGraph.png)

<div align="center" style="color:gold;"><strong>Don't forget to give a ⭐ if you found this helpful.</strong></div><br>

Revolutionizing website internal linking by leveraging cutting-edge data processing techniques, vector embeddings, and graph-based link prediction algorithms. By combining these advanced technologies and methodologies, the project aims to create an intelligent solution that optimizes internal link structures, enhancing both SEO performance and user navigation.

We're enabling **the first publicly available and transparent research for academic and industry purposes in the field of end-to-end SEO and technical marketing on a global level**. This initiative opens the door to innovation and collaboration, setting a new standard for how large-scale websites can manage and improve their internal linking strategies using AI-powered, reproducible methods. **A scientific paper is in progress and will follow.**

**Note:** We’ve implemented clearer separation between frontend, backend, testing, and data logic, and are now conducting **rigorous stress tests** with the SEO community.

---
<h1 align="center">
    Quick Tour
</h1>

<h3 align="center">
    <a href="#-target-reading-audience">Target Audience</a> &bull;
    <a href="#-sponsors">Sponsors</a> &bull;
    <a href="#️-getting-started">Getting Started</a> &bull; <br>
    <a href="#-app-uis">App UIs</a> &bull;
    <a href="#%EF%B8%8F-product-roadmap">Product Roadmap</a> &bull;
    <a href="#-license">License</a> &bull;
</h3>

---
# 📂 Project Structure

The project is organized into a modular structure to promote maintainability, reusability, and clear separation of concerns. This is the current folder layout but can change over time:

```
WebKnoGraph/ (Project Root)
├── .github/
│   └── workflows/
│       ├── lint_and_format.yaml
│       └── python_tests.yaml
├── assets/
│   ├── 03_link_graph.png
│   ├── 04_graphsage_01.png
│   ├── 04_graphsage_02.png
│   ├── bmc-brand-logo.png
│   ├── crawler_ui.png
│   ├── embeddings_ui.png
│   ├── fcse_logo.png
│   ├── internal-linking-seo-roi-cropped.png
│   ├── kalicube.com.png
│   ├── pagerank_ui.png
│   ├── product_roadmap.png
│   ├── test_completed_1.png
│   ├── test_completed_2.png
│   ├── WebKnoGraph.png
│   └── WL_logo.png
├── data/
│   ├── crawled_data_parquet/
│   │   └── crawl_date=2025-06-28/
│   ├── prediction_model/
│   │   ├── edge_index.pt
│   │   ├── final_node_embeddings.pt
│   │   ├── graphsage_link_predictor.pth
│   │   └── model_metadata.json
│   ├── url_embeddings/
├── notebooks/
│   ├── automatic_link_recommendation_ui.ipynb
│   ├── crawler_ui.ipynb
│   ├── embeddings_ui.ipynb
│   ├── link_crawler_ui.ipynb
│   ├── link_prediction_ui.ipynb
│   └── pagerank_ui.ipynb
├── results/
│   ├── automatic_led/
│   │   ├── folder_batches/
│   │   ├── high_batches/
│   │   ├── high_boosters/
│   │   ├── low_batches/
│   │   ├── mixed_batches/
│   │   └── random_batches/
│   ├── base_file_types/
│   ├── expert_led/
│   │   ├── folder_batches/
│   │   ├── high_batches/
│   │   ├── low_batches/
│   │   ├── mixed_batches/
│   │   └── random_batches/
├── src/
│   ├── backend/
│   │   ├── config/
│   │   ├── data/
│   │   ├── graph/
│   │   ├── models/
│   │   ├── services/
│   │   ├── utils/
│   │   └── __init__.py
│   └── shared/
│       ├── __init__.py
│       ├── interfaces.py
│       └── logging_config.py
├── tests/
│   ├── backend/
│   │   ├── services/
│   │   └── __init__.py
│   └── __init__.py
├── .gitignore
├── .pre-commit-config.yaml
├── CHANGELOG.md
├── CITATION.cff
├── generate_structure_insightful.py
├── HOW-IT-WORKS.md
├── LICENSE
├── README.md
├── requirements.txt
└── trim_ws.py
```

## Starting a Fresh Crawl

To begin a new crawl for a different website, delete the entire `data/` folder. This directory stores all intermediate and final outputs from the previous crawl session. Removing it ensures a clean start without residual data interfering.

### Contents of the `data/` Directory

| Path | Description |
|------|-------------|
| `data/` | Root folder for all crawl-related data and model artifacts. |
| `data/link_graph_edges.csv` | Stores inter-page hyperlinks, forming the basis of the internal link graph. |
| `data/url_analysis_results.csv` | Contains extracted structural features such as PageRank and folder depth per URL. |
| `data/crawled_data_parquet/` | Directory for the raw HTML content captured by the crawler in Parquet format. |
| `data/crawler_state.db` | SQLite database that maintains the crawl state to support resume capability. |
| `data/url_embeddings/` | Holds vector embeddings representing the semantic content of each URL. |
| `data/prediction_model/` | Includes the trained GraphSAGE model and metadata for link prediction. |

For additional details about how this fits into the full project workflow, refer to the [Project Structure](#-project-structure) section of the README.

---
# 💪 Sponsors

We are incredibly grateful to our sponsors for their continued support in making this project possible. Their contributions have been vital in pushing the boundaries of what can be achieved through data-driven internal linking solutions.

- **WordLift.io**: We extend our deepest gratitude to [WordLift.io](https://wordlift.io/) for their generous sponsorship and for sharing insights and data that were essential for this project's success.
- **Kalicube.com**: Special thanks to [Kalicube.com](https://kalicube.com/) for providing invaluable data, resources, and continuous encouragement. Your support has greatly enhanced the scope and impact of WebKnoGraph.
- **Faculty of Computer Science and Engineering (FCSE) Skopje**: A heartfelt thanks to [FCSE Skopje professors, PhD Georgina Mircheva and PhD Miroslav Mirchev](https://www.finki.ukim.mk/en) for their innovative ideas and technical suggestions. Their expertise and advisory during this were a key component in shaping the direction of WebKnoGraph.

Without the contributions from these amazing sponsors, WebKnoGraph would not have been possible. Thank you for believing in the vision and supporting the evolution of this groundbreaking project.

<p align="center">
  <img src="https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/WL_logo.png" height="70"/>&nbsp;&nbsp;
  <img src="https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/kalicube.com.png" height="70"/>&nbsp;&nbsp;&nbsp;
  <img src="https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/fcse_logo.png" height="70"/>
</p>

---
# 📷 App UIs

The project is composed of six modules, illustrated in the images below.

## 1. WebKnoGraph Crawler
![WebKnoGraph Crawler](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/crawler_ui.png)

## 2. Embeddings Generator
![Embeddings Controller](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/embeddings_ui.png)

## 3. LinkGraph Extractor
![LinkGraph Extractor](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/03_link_graph.png)

## 4. HITS and PageRank URL Sorter
![PageRank and HITS Sorted URLs](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/pagerank_ui.png)

## 5. GNN Model Trainer
![Train GNN Algo](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/04_graphsage_01.png)

## 6. Link Prediction Engine

### Delete runtime and re-run the script from step 5
![Link Prediction Engine](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/04_graphsage_02.png)

---
We welcome more sponsors and partners who are passionate about driving innovation in SEO and website optimization. If you're interested in collaborating or sponsoring, feel free to reach out!

---
# 👐 Who is WebKnoGraph for?

WebKnoGraph is created for companies where content plays a central role in business growth. It is suited for mid to large-sized organizations that manage high volumes of content, often exceeding 1,000 unique pages within each structured folder, such as a blog, help center, or product documentation section.

These organizations publish regularly, with dedicated editorial workflows that add new content across folders, subdomains, or language versions. Internal linking is a key part of their SEO and content strategies. However, maintaining these links manually becomes increasingly difficult as the content volume grows.

WebKnoGraph addresses this challenge by offering AI-driven link prediction workflows. It supports teams that already work with technical SEO, semantic search, or structured content planning. It fits well into environments where companies prefer to maintain direct control over their data, models, and optimization logic rather than relying on opaque external services.

The tool is especially relevant for the following types of companies:

1. **Media and Publishing Groups**:
   Teams operating large-scale news websites, online magazines, or niche vertical content hubs.

2. **B2B SaaS Providers**:
   Companies managing growing knowledge bases, release notes, changelogs, and resource libraries.

3. **Ecommerce Brands and Marketplaces**:
   Organizations that handle thousands of product pages, category overviews, and search-optimized content.

4. **Enterprise Knowledge Platforms**:
   Firms supporting complex internal documentation across departments or in multiple languages.

WebKnoGraph empowers these organizations to scale internal linking with precision, consistency, and clarity, while keeping full control over their infrastructure.

---
# 📖 Target Reading Audience

WebKnoGraph is designed for **tech-savvy marketers and marketing engineers** with a strong understanding of advanced data analytics and data-driven marketing strategies. Our ideal users are professionals who have experience with Python or have access to development support within their teams.

These individuals are skilled in interpreting and utilizing data, as well as working with technical tools to optimize internal linking structures, improve SEO performance, and enhance overall website navigation. Whether directly coding or collaborating with developers, they are adept at leveraging data and technology to streamline marketing operations, increase customer engagement, and drive business growth.

If you're a data-driven marketer comfortable with using cutting-edge tools to push the boundaries of SEO, WebKnoGraph is built for you.

---
# ⚡️ Getting Started

To explore and utilize WebKnoGraph, follow the instructions below to get started with the code, data, and documentation provided in the repository:

- **Code**: The core code for this project is located in the `src` folder, organized into `backend` and `shared` modules. The `notebooks` folder contains the Jupyter notebooks that serve as interactive Gradio UIs for each application.
- **Data**: The data used for analysis and testing, as well as generated outputs (like crawled content, embeddings, and link graphs), are stored within the `data` folder (though this folder is typically empty in the repository and populated at runtime).
- **Technical Report**: For a comprehensive understanding of the project, including the methodology, algorithms, and results, refer to the detailed technical report provided in the `technical_report/WebKnoGraph_Technical_Report.pdf` file. This document gives an in-depth coverage of the concepts and the execution of the solution.

By following these resources, you will gain full access to the materials and insights needed to experiment with and extend WebKnoGraph.

---

# 🚀 Setup and Running

This project is designed to be easily runnable in a Google Colab environment, leveraging Google Drive for persistent data storage.

## 1. Prerequisites

* **Google Account:** Required for Google Colab and Google Drive.
* **Python 3.8+**

## 2. Clone/Upload the Repository

1. **Clone (if using Git locally):**
   ```bash
   git clone https://github.com/martech-engineer/WebKnoGraph.git
   cd WebKnoGraph
   ```
   Then, upload this `WebKnoGraph` folder to your Google Drive.

2. **Upload (if directly from Colab):**
   * Download the entire `WebKnoGraph` folder as a ZIP from the repository.
   * Unzip it.
   * Upload the `WebKnoGraph` folder directly to your Google Drive (e.g., into `My Drive/`). Ensure the internal folder structure is preserved exactly as shown in the [Project Structure](#-project-structure) section.

## 3. Google Drive Mounting

All notebooks assume your `WebKnoGraph` project is located at `/content/drive/My Drive/WebKnoGraph/` after Google Drive is mounted. This path is explicitly set in each notebook.

Each notebook's first cell contains the necessary Python code to mount your Google Drive. You will be prompted to authenticate.

```python
# Part of the first cell in each notebook
from google.colab import drive

drive.mount("/content/drive", force_remount=True)
```

## 4. Install Dependencies

Each notebook's first cell also contains commented-out `!pip install` commands. It's recommended to:

1. Open any of the notebooks (e.g., `notebooks/crawler_ui.ipynb`).
2. Uncomment the `!pip install ...` lines in the first cell.
3. Run that first cell. This will install all necessary libraries into your Colab environment for the current session. Alternatively, you can manually run `!pip install -r requirements.txt` in a Colab cell, ensuring your requirements.txt is up to date.

5. Running the Applications (Gradio UIs)

Each module has its own dedicated Gradio UI notebook. It's recommended to run them in the following order as outputs from one serve as inputs for the next.
General Steps for Each Notebook:
* Open the desired `*.ipynb` file in Google Colab.
* Go to `Runtime` -> `Disconnect and delete runtime` (This is **CRUCIAL** for a clean start and to pick up any code changes).
* Go to `Runtime` -> `Run all cells`.
* After the cells finish executing, a Gradio UI link (local and/or public `ngrok.io` link) will appear in the output of the last cell. Click this link to interact with the application.

5.1. Content Crawler

* **Notebook:** `notebooks/crawler_ui.ipynb`
* **Purpose:** Crawl a website and save content as Parquet files.
* **Default Output:** `/content/drive/My Drive/WebKnoGraph/data/crawled_data_parquet/`

5.2. Embeddings Pipeline

* **Notebook:** `notebooks/embeddings_ui.ipynb`
* **Purpose:** Generate embeddings for crawled URLs.
* **Requires:** Output from the Content Crawler (`crawled_data_parquet/`).
* **Default Output:** `/content/drive/My Drive/WebKnoGraph/data/url_embeddings/`

5.3. Link Graph Extractor

* **Notebook:** `notebooks/link_crawler_ui.ipynb`
* **Purpose:** Extract internal FROM, TO links and save as a CSV edge list.
* **Default Output:** `/content/drive/My Drive/WebKnoGraph/data/link_graph_edges.csv`

5.4. GNN Link Prediction & Recommendation Engine

* **Notebook:** `notebooks/link_prediction_ui.ipynb`
* **Purpose:** Train a GNN model on the link graph and embeddings, then get link recommendations.
* **Requires:**
    * Output from Link Graph Extractor (`link_graph_edges.csv`).
    * Output from Embeddings Pipeline (`url_embeddings/`).
* **Default Output:** `/content/drive/My Drive/WebKnoGraph/data/prediction_model/`
* **Important Note:** After training, you must select a specific URL from the dropdown in the "Get Link Recommendations" tab for recommendations to be generated. Do not use the placeholder message.

5.5. PageRank & HITS Analysis

* **Notebook:** `notebooks/pagerank_ui.ipynb`
* **Purpose:** Calculate PageRank and HITS scores for URLs based on the link graph, and analyze folder depths.
* **Requires:** Output from the Link Graph Extractor (`link_graph_edges.csv`). (It also generates `url_analysis_results.csv` which is then used internally for HITS analysis).
* **Default Output:** `/content/drive/My Drive/WebKnoGraph/data/url_analysis_results.csv`

**Important Note:** After training, you must select a specific URL from the dropdown in the "Get Link Recommendations" tab for recommendations to be generated. Do not use the placeholder message.

## 6. Running All Tests in Your Project

To execute all unit tests located within the tests/backend/services/ directory and its subdirectories, navigate to the root of your WebKnoGraph project in your terminal. Once there, you can use Python's built-in unittest module with its discover command:

```bash
   python -m unittest discover tests/backend/services/
   ```

### Understanding the Output

*   **python -m unittest**: This part invokes the unittest module as a script.

*   **discover**: This command tells unittest to search for and load all test cases.

*   **tests/backend/services/**: This specifies the starting directory for the test discovery process. unittest will look for any file whose name begins with test (e.g., test\_crawler\_service.py, test\_pagerank\_service.py) within this directory and any subdirectories, and then run all test methods found within the unittest.TestCase classes in those files.


A successful test run will typically show a series of dots (.) indicating passed tests. If any tests fail (F) or encounter errors (E), they will be clearly marked, and a summary of the failures/errors will be provided at the end of the output.

### Example of Successful Test Output

![GitHub Actions Test Validation](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/test_completed_1.png)

![Bash Test Validation](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/test_completed_2.png)

This output confirms that all tests in the tests/backend/services/ directory were found and executed, and the final summary will indicate if all of them passed successfully.

---
# ⚠️ Troubleshooting Tips

## ModuleNotFoundError: No module named 'src':
- Ensure your `WebKnoGraph` folder is directly under `/content/drive/My Drive/`.
- Verify that `src` directory exists within `WebKnoGraph` and contains `backend/` and `shared/`.
- Make sure the `project_root` variable in the first cell of your notebook exactly matches the absolute path to your `WebKnoGraph` folder on Google Drive.
- Always perform a **Runtime -> Disconnect and delete runtime** before re-running.

## ModuleNotFoundError: No module named 'src.backend.data.some_module' (or similar):
- Check your file paths (`!ls -R "/content/drive/My Drive/WebKnoGraph"`) to ensure the module file (`some_module.py`) is physically located at the path implied by the import (`src/backend/data/`).
- Ensure there's an `__init__.py` file (even if empty) in every directory along the import path (e.g., `src/backend/__init__.py`, `src/backend/data/__init__.py`).
- Verify the exact case-sensitivity of folder and file names.
- Confirm you have copy-pasted the entire content into the file and saved it correctly. An empty or syntax-error-laden file will also cause this.
- Always perform a **Runtime -> Disconnect and delete runtime** before re-running.

## ImportError: generic_type: type "ExplainType" is already registered!" (for duckdb):
- This typically indicates a conflict from multiple installations or an unclean session.
- Perform a **Runtime -> Disconnect and delete runtime** and then run all cells from scratch. Ensure the `!pip install` commands run in the very first cell before any other imports.

## KeyError in RecommendationEngine / Dropdown Issues:
- Ensure the model training pipeline completes successfully first.
- After training, manually select a valid URL from the dropdown for recommendations. The dropdown might initially show a placeholder if artifacts don't exist.
- If retraining, ensure old output artifacts are cleared or overwritten.

---
# 🗺️ Product Roadmap

This roadmap outlines the planned feature development and research milestones for WebKnoGraph across upcoming quarters. It is organized around key strategic themes: algorithmic enhancements, deployment, testing, user interface customization, and research paper work. Each milestone reflects a step toward building a robust, AI-driven system for optimizing internal linking at scale.

![Product Roadmap](https://github.com/martech-engineer/WebKnoGraph/blob/main/assets/product_roadmap.png)

---
# 🤝 Contributing

WebKnoGraph invites contributions from developers, researchers, marketers, and anyone driven by curiosity and purpose. This project evolves through collaboration.

You can contribute by improving the codebase, refining documentation, testing workflows, or proposing new use cases. Every pull request, idea, and experiment helps shape a more open and intelligent future for SEO and internal linking.

Clone the repo, start a branch, and share your expertise. Progress happens when people build together.

---
# 📄 License

WebKnoGraph is released under the **Apache License 2.0**.

This license allows open use, adaptation, and distribution. You can integrate the project into your own workflows, extend its functionality, or build on top of it. The license ensures the project remains accessible and reusable for individuals, teams, and institutions working at the intersection of SEO, AI, and web infrastructure.

Use the code. Improve the methods. Share what you learn.

---

## 🖩 Internal Linking Calculator

This interactive calculator estimates the potential **cost savings and ROI** from optimizing internal links, based on your keyword data, CPC benchmarks, and click-through assumptions.

[![Try the Internal Linking SEO ROI Calculator](https://raw.githubusercontent.com/martech-engineer/WebKnoGraph/refs/heads/main/assets/internal-linking-seo-roi-cropped.png)](https://huggingface.co/spaces/Em4e/internal-linking-seo-roi-calculator)

---

## Citation

If you use WebKnoGraph in your research, please cite our arXiv preprint. The work is currently available as a preprint while we await peer review.

```bibtex
@misc{gjorgjevska2026webknograph,
  title         = {WebKnoGraph: GNN-Powered Internal Linking},
  author        = {Gjorgjevska, Emilija and Mirceva, Georgina and Mirchev, Miroslav},
  year          = {2026},
  eprint        = {2606.06106},
  archivePrefix = {arXiv},
  primaryClass  = {cs.IR},
  url           = {https://arxiv.org/abs/2606.06106}
}
