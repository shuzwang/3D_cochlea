# 3D cochlea

It contains the analytical code to understand the developmental mechanisms of mouse cochlear duct using scRNA-seq datasets from E12.5 and E14.5 two time points. 

# Abstract
In the mammalian auditory system, frequency discrimination depends on morphological and physiological properties of the organ of Corti that gradually change along the longitudinal (tonotopic) axis of the organ and therefore shape the tuning properties of individual hair cells. At the molecular level, those frequency-specific characteristics are mirrored in gene expression gradients, which require tonotopic patterning of the cochlea. To infer molecular mechanisms, we reconstructed the embryonic cochlear duct in 3D space from single-cell gene expression data and proposed two hypotheses regarding spatial patterning. Analyzing scRNA-seq data from two developmental time points suggested that morphogens, rather than a timing-related mechanism confer spatial identity in the cochlea. Subsequently, retinoic acid and sonic hedgehog pathways were identified to form opposing tonotopic gradients and functional interrogation utilizing cochlear explants suggested that both pathways jointly specify the tonotopic axis during development of the murine organ of Corti.

# Analytical Pipelines
Here we include analytical pipeline code for our manuscript.
* Seurat v3 pipeline: Seurat v3 pipeline is to analyze scRNA-seq datasets from both E12.5 and E14.5 time points. We considered E12.5 and E14.5 datasets separately. For each dataset, we conducted QC steps, identified clusters, annotated cell type for each cluster using canonical marker genes, and determined differentially expressed genes for each cell type. We followed the documentations from https://satijalab.org/seurat/.
* LIGER pipeline: LIGER pipeline is to joint analyze E12.5 and E14.5 scRNA-seq datasets. We co-embedded these two datasets onto the same UMAP and validated the identified clusters from `Seurat v3 pipeline`. We followed the documentation of LIGER from http://htmlpreview.github.io/?https://github.com/welch-lab/liger/blob/master/vignettes/Integrating_multi_scRNA_data.html. 
* CellTrails pipeline: CellTrails pipeline is to determine sub-populations of the cochlear duct. Here, we concentrated on cochlear roof cells and cochlear floor cells, which compose of cochlear apex cells and cochlear base cells. E12.5 and E14.5 datasets were considered separately. To rule out the cell cycle effects, we filtered out the cell cycle genes. For each dataset, we conducted dimension reduction using spectral embedding to identify sub-populations of the cochlea, which results in 10 and 14 sub-populations of the cochlea for E12.5 and E14.5 data, respectively. Furthermore, we aggreated the subpopulations into 6 meta clusters for each time point by conducting hierarchical clustering analysis and annotated the metaclusters based on the differentially expressed genes. Hair cells and supporting cells were removed from our analysis due to their unique transcriptome profiles and limited cell number. This pipeline prepared the datasets and annotations for 2D/3D cochlea reconstruction. We followed the documentation from https://hellerlab.stanford.edu/celltrails/. 
* AUCell pipeline: AUCell is to calculate gene set enrichment score on single-cell resolution. AUCell, a ranking-based method, uses the “Area Under the Curve” (AUC) of the recovery curve to determine the enrichment of GO terms on individual cells. GO terms with at least 4 genes related were subjected for further analysis. E12.5 and E14.5 datasets were considered separately. First, we selected cells annotated as cochlear duct, including cochlear floor and cochlear roof cells, and then ranked all genes for each cell using the function *AUCell_buildRanking* with default settings. Next, AUC for each GO term for each cell was calculated using *AUCell_calcAUC* function, and top 10% of the genes in the ranking were used. To identify the differentially enriched pathways between cochlear apex and cochlear base, we selected the corresponding cells and conducted Wilcoxon sum-rank test with Benjamini-Hochberg (BH) correction (*P*-adjust < 0.05) for E12.5 and E14.5 datasets, respectively. Similarly, we determined differential pathways between cochlear roof and cochlear floor cells. 


# Novel Analysis

Besides the existing packages we used, we also generated some novel functions for our manuscript. 
* 2D/3D reconstruction:  
* Gradually expressed gene identification:
* Similarity method to test two hypotheses about developing cochlea extension:
* 

