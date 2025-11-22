
# 🧬 Single-Cell RNA-seq Analysis with Scanpy

This notebook walks through a complete single-cell RNA-seq (scRNA-seq) data analysis pipeline using the Python package Scanpy.The notebook can be run in Google Colab.
It includes following steps:
    Installation of Dependencies
    Loading and preprocessing data
    Quality control (QC)
    Normalization and feature selection
    Dimensionality reduction (PCA, UMAP)
    Clustering
    Marker gene detection and visualization
## Installation of Dependencies 
 Scanpy
 anndata
 igraph
 celltypist
 decoupler
 fa2-modified
 ## 🧩 Loading Data
The single-cell expression matrix is loaded into an AnnData object — the core data structure in Scanpy. It contains:

    adata.X: the expression matrix (cells × genes)
    adata.obs: metadata for each cell
    adata.var: metadata for each gene
## 🧹 Quality Control (QC)
QC ensures only high-quality cells and informative genes are kept. Typical filters remove:

    Harmonize unique gene names (avoid gene duplications from old pipelines)
    Cells with too few genes (likely dead)
    Cells with too many genes (possible doublets)
    Genes expressed in very few cells (uninformative)
## ⚖️ Normalization

Normalization adjusts for sequencing depth differences between cells. Here, we scale counts so each cell has the same total expression level.

## 🔍 Dimensionality Reduction (PCA)

Principal Component Analysis (PCA) is used to reduce data complexity and highlight key variation patterns. This makes later steps like clustering and visualization faster and more robust.

In single-cell RNA-seq, each cell has expression values for thousands of genes, creating a huge, noisy matrix. PCA compresses this high-dimensional data into a smaller set of features (typically 30–50 components) that summarize the key biological and technical variation across cells.

    Noise reduction: scRNA-seq data are sparse and noisy. PCA focuses on the strongest correlated gene expression patterns, discarding random noise.

    Computational efficiency: Downstream analyses like clustering, UMAP, or t-SNE run much faster and more robustly on 30 PCs than on 20,000 genes.

    Signal extraction: The top PCs often correspond to meaningful biological structure—cell type, cell cycle state, or activation level—while later PCs capture less relevant variation.
    
## Clustering by communities.

Clustering by communities in single-cell RNA-seq is the process of grouping cells that show similar expression profiles — essentially, discovering putative cell types or states.Once PCA compresses your data into a manageable set of dimensions, clustering algorithms like Leiden operate on a graph-based representation of cell–cell relationships.It is used for cell type detection

## Marker gene detection and visualization -Cell Annotation

Cell annotation is the process of assigning biological meaning—like cell type or functional state—to each cluster found after Leiden clustering.

Traditionally, this relies on manual marker gene inspection: identifying top genes per cluster and match them to known markers. Decoupler is used enable a more systematic and data-driven approach.

Decoupler is a framework for gene set activity inference. Instead of labeling clusters by single markers, it estimates the activity of predefined pathways, transcription factors, or cell-type signatures from known databases (e.g., MSigDB, PROGENy, DoRothEA).

In practice:

    A normalized expression matrix (adata) is provided.

    Gene sets representing known biological programs or cell-type signatures are loaded.

    Decoupler calculates an activity score per cell or cluster using methods like weighted mean, ULM, or AUCell.

    Activity scores are interpreted to annotate clusters automatically or semi-automatically.





