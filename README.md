🧠 Botryllus Brain scRNA-seq Analysis
This repository contains a single-cell RNA-sequencing (scRNA-seq) analysis of Botryllus schlosseri brain tissue. The workflow includes quality control, normalization, dimensionality reduction, clustering, and visualization.

📊 Dataset Overview
Initial shape: 683 cells × 44,727 genes

Post-QC and normalization: 581 cells × 14,658 genes

🔬 Analysis Workflow
1. Quality Control & Normalization
Removed low-quality cells and low-expression genes.

Identified highly variable genes based on gene dispersion.

2. Dimensionality Reduction (PCA)
Conducted PCA using highly variable genes.

Visualizations:

PC1 vs. PC2 colored by anatomical region (e.g., AB, B2)

Scree plot of variance ratio per principal component

Top gene contributors to PC1 and PC2

3. UMAP Embedding
Visualized cell structure in 2D using UMAP with different distance metrics:

Cosine: n_neighbors=25, n_pcs=5

Euclidean: n_neighbors=25, n_pcs=5

These revealed distinct groupings among cell populations.

4. Clustering
Applied two clustering methods on UMAP-embedded data:

Leiden Clustering

Cosine metric: resolution=0.58

Euclidean metric: resolution=0.5

DBSCAN Clustering

Cosine metric: eps=0.00045, min_samples=5

Euclidean metric: eps=0.6, min_samples=5

Each clustering strategy revealed distinct and interpretable cell clusters.

