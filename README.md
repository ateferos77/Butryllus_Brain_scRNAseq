\documentclass[11pt]{article}
\usepackage[a4paper, margin=1in]{geometry}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{amsmath}
\usepackage{enumitem}

\title{\textbf{Botryllus Brain scRNA-seq Analysis}}
\author{}
\date{}

\begin{document}

\maketitle

\section*{Overview}

This document summarizes the single-cell RNA-sequencing (scRNA-seq) analysis conducted on \textit{Botryllus schlosseri} brain tissue. The workflow includes quality control, normalization, dimensionality reduction, clustering, and visualization.

\section*{Dataset Summary}

\begin{itemize}[leftmargin=1.5em]
    \item \textbf{Initial shape:} 683 cells $\times$ 44,727 genes
    \item \textbf{Post-QC and normalization:} 581 cells $\times$ 14,658 genes
\end{itemize}

\section*{Analysis Workflow}

\subsection*{1. Quality Control \& Normalization}
\begin{itemize}[leftmargin=1.5em]
    \item Removed low-quality cells and genes with low expression.
    \item Identified highly variable genes using mean-variance relationships.
\end{itemize}

\subsection*{2. Dimensionality Reduction (PCA)}
\begin{itemize}[leftmargin=1.5em]
    \item Applied PCA on highly variable genes.
    \item Visualizations included:
    \begin{itemize}
        \item PC1 vs. PC2 scatter plot, colored by anatomical regions (e.g., AB, B2).
        \item Variance ratio plot across PCs.
        \item Top gene loadings for PC1 and PC2.
    \end{itemize}
\end{itemize}

\subsection*{3. UMAP Embedding}
\begin{itemize}[leftmargin=1.5em]
    \item Visualized data using UMAP with different distance metrics:
    \begin{itemize}
        \item \textbf{Cosine:} $n\_neighbors=25$, $n\_pcs=5$
        \item \textbf{Euclidean:} $n\_neighbors=25$, $n\_pcs=5$
    \end{itemize}
    \item UMAP projections revealed clear population structures.
\end{itemize}

\subsection*{4. Clustering}
\begin{itemize}[leftmargin=1.5em]
    \item Clustering was performed using:
    \begin{description}
        \item[Leiden Clustering:]
        \begin{itemize}
            \item Cosine: \texttt{resolution = 0.58}
            \item Euclidean: \texttt{resolution = 0.5}
        \end{itemize}
        \item[DBSCAN Clustering:]
        \begin{itemize}
            \item Cosine: \texttt{eps = 0.00045}, \texttt{min\_samples = 5}
            \item Euclidean: \texttt{eps = 0.6}, \texttt{min\_samples = 5}
        \end{itemize}
    \end{description}
    \item Different algorithms and metrics revealed distinct but consistent clusters.
\end{itemize}

\section*{Next Steps (Optional)}
If applicable, further analyses may include:
\begin{itemize}[leftmargin=1.5em]
    \item Marker gene identification
    \item Cluster annotation
    \item Differential expression analysis
    \item Cell type inference
\end{itemize}

\end{document}
