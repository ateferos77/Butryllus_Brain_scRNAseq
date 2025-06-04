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

### 5. Clustering Quality Metrics

To evaluate the quality of our clustering results, we applied several internal metrics: **Silhouette Score**, **Calinski-Harabasz Index**, and **Davies-Bouldin Index**. These metrics offer insights into how well-defined and meaningful the clusters are.

#### Silhouette Score (Range: -1 to 1)
- Measures how well-separated the clusters are. A higher value indicates better separation between clusters.

#### Calinski-Harabasz Index (Higher is Better)
- Indicates how compact and well-separated clusters are based on variance ratios.

#### Davies-Bouldin Index (Lower is Better)
- Measures the average similarity between clusters. Lower values indicate that the clusters are more distinct and less overlapping.

**Clustering Method Results:**

| Clustering Method                        | Silhouette Score | Calinski-Harabasz Index | Davies-Bouldin Index |
|------------------------------------------|------------------|-------------------------|----------------------|
| leiden_res0.58_nn25_pcs5_cosine         | 0.115            | 28.478                  | 2.123                |
| dbscan_eps0.00045_min5_nn25_pcs5_cosine  | 0.006            | 19.205                  | 2.903                |
| leiden_res0.5_nn25_pcs7_euclidean       | 0.139            | 29.847                  | 2.367                |
| dbscan_eps0.6_min5_nn25_pcs7_euclidean  | 0.078            | 25.525                  | 2.411                |

**Interpretation:**
- **Leiden clustering methods** show consistently better quality, with higher silhouette scores, better Calinski-Harabasz scores, and lower Davies-Bouldin scores. This suggests that Leiden clustering produces better-separated, more compact clusters than DBSCAN.
- **DBSCAN** shows weaker performance, especially with very low epsilon values, which likely led to over-tight clustering and poor separation of cells.

### 6. Comparison of Clustering Results Using ARI and NMI

Next, we assessed the agreement between different clustering methods using the **Adjusted Rand Index (ARI)** and **Normalized Mutual Information (NMI)**. These metrics compare how similar two clustering results are, with higher values indicating stronger agreement.

**ARI and NMI Results:**

| Cluster Pair                              | ARI    | NMI    |
|-------------------------------------------|--------|--------|
| leiden_res0.58 vs dbscan_eps0.00045      | 0.631  | 0.759  |
| leiden_res0.58 vs leiden_res0.5          | 0.746  | 0.835  |
| leiden_res0.58 vs dbscan_eps0.6         | 0.669  | 0.797  |
| dbscan_eps0.00045 vs leiden_res0.5      | 0.659  | 0.757  |
| dbscan_eps0.00045 vs dbscan_eps0.6      | 0.569  | 0.714  |
| leiden_res0.5 vs dbscan_eps0.6          | 0.757  | 0.876  |

**Interpretation:**
- The highest agreement is found between **leiden_res0.5** and **dbscan_eps0.6**, showing that these methods produce very similar cluster assignments.
- **Leiden methods** (res0.58 and res0.5) show strong agreement, reflecting consistency within the Leiden clustering approach, even with slight parameter variations.
- **DBSCAN methods** with different epsilon values show moderate agreement but less similarity than Leiden, indicating that DBSCAN's clustering is more sensitive to its parameter settings.

### 7. Cell Distribution Among Clusters

We examined how cells were distributed across the identified clusters. For the **leiden_res0.58_nn25_pcs5_cosine** clustering, the distribution of cells across clusters was as follows:

| Cluster | Number of Cells |
|---------|-----------------|
| 0       | 108             |
| 1       | 91              |
| 2       | 87              |
| 3       | 73              |
| 4       | 58              |
| 5       | 55              |
| 6       | 38              |
| 7       | 28              |
| 8       | 25              |
| 9       | 18              |

The cluster sizes varied significantly, with the largest cluster containing 108 cells, and the smallest containing only 18 cells. This distribution will be useful for subsequent analyses, such as identifying marker genes and examining biological differences across clusters.

