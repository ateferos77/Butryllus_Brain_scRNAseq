# 🧠 Botryllus Brain scRNA-seq Analysis

This repository contains a single-cell RNA-sequencing (scRNA-seq) analysis of **Botryllus schlosseri** brain tissue. The workflow includes quality control, normalization, dimensionality reduction, clustering, and visualization.

---

## 📊 Dataset Overview

- **Initial shape:** 683 cells × 44,727 genes  
- **Post-QC and normalization:** 581 cells × 14,658 genes  

---

## 🔬 Analysis Workflow

### 1. Quality Control & Normalization

- Removed low-quality cells and genes with low expression using threshold-based filtering.
- Retained genes that showed sufficient dispersion across cells to focus on biologically informative signals.
- Performed **library size normalization** to correct for differences in sequencing depth across cells.
- Applied **log-transformation** to stabilize variance and make gene expression values more comparable.
- Identified **highly variable genes** to be used in downstream dimensionality reduction and clustering.

### 2. Dimensionality Reduction (PCA)

- Applied PCA to reduce dimensionality based on highly variable genes.
- Visualizations include:
  - **PC1 vs. PC2** scatter plot colored by anatomical region (e.g., AB vs. B2)
  - Scree plot of **explained variance ratio** per component
  - **Top contributing genes** to PC1 and PC2

### 3. UMAP Embedding

- Visualized the dataset structure in 2D using UMAP:
  - **Cosine metric:**  
    ```
    sc.pp.neighbors(adata, n_neighbors=25, n_pcs=5, random_state=0, metric='cosine')
    ```
  - **Euclidean metric:**  
    ```
    sc.pp.neighbors(adata, n_neighbors=25, n_pcs=5, random_state=0, metric='euclidean')
    ```
- UMAP projections revealed distinct cell populations and spatial patterns.

### 4. Clustering

To identify distinct cell populations, two clustering algorithms were applied: **Leiden** and **DBSCAN**. Each algorithm was tested using UMAP embeddings generated from two distance metrics: **cosine** and **euclidean**.

- For the UMAP generated using **cosine distance**:
  - The **Leiden algorithm** was used with a resolution of 0.58.
  - The **DBSCAN algorithm** was applied with an epsilon value of 0.00045 and a minimum of 5 samples per cluster.

- For the UMAP generated using **euclidean distance**:
  - The **Leiden algorithm** was applied with a resolution of 0.5.
  - The **DBSCAN algorithm** was used with an epsilon value of 0.6 and a minimum of 5 samples.

These clustering methods revealed consistent and biologically interpretable groupings of cells. Leiden produced well-separated, compact clusters, while DBSCAN captured finer substructures and identified potential outliers. This provided a robust foundation for downstream analyses such as marker gene identification and cell type annotation.
