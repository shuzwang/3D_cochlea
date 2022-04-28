# 3D cochlea

It contains the analytical code to understand the developmental mechanisms of mouse cochlear duct using scRNA-seq datasets from E12.5 and E14.5 two time points. 

# Abstract

# Analytical Pipelines
Here we include analytical pipeline code for our manuscript.
* Seurat v3 pipeline: Seurat v3 pipeline is to analyze scRNA-seq datasets from both E12.5 and E14.5 time points. We considered E12.5 and E14.5 datasets separately. For each dataset, we conducted QC steps, identified clusters, annotated cell type for each cluster using marker genes, and determined differentially expressed genes for each cell type. We followed the documentations from https://satijalab.org/seurat/.
* LIGER pipeline: LIGER pipeline is to joint analyze E12.5 and E14.5 scRNA-seq datasets. We co-embedded these two datasets onto the same UMAP and validated the identified clusters from `Seurat v3 pipeline`. We followed the documentation of LIGER from http://htmlpreview.github.io/?https://github.com/welch-lab/liger/blob/master/vignettes/Integrating_multi_scRNA_data.html. 
* CellTrails pipeline: CellTrails pipeline is to determine sub-populations of the cochlear duct. Here we concentrated on cochlear roof cells and cochlear floor cells, which compose of cochlear apex cells and cochlear base cells. E12.5 and E14.5 datasets were considered separately. For each dataset, we conducted dimention reduction using spectral embedding, and run hierarchical spectral clustering to identify sub-populations of the cochlea, which results in 10 and 14 sub-populations of the cochlea for E12.5 and E14.5 data, respectively. Furthermore, we generated 6 meta clusters by conducting hierarchical cluster analysis and annotated the metaclusters based on the differentially expressed genes for both datasets. This pipeline prepared the datasets and annotations for 2D/3D cochlea reconstruction. We followed the documentation from https://hellerlab.stanford.edu/celltrails/. 
* AUCell pipeline: 

# Novel Analysis

Besides the existing packages we used, we also generated some novel functions for our manuscript. 
* 2D/3D reconstruction:  
* Gradually expressed gene identification:
* 

