# 3D cochlea

It contains the analytical code to understand the developmental mechanisms of mouse cochlear duct using scRNA-seq datasets from E12.5 and E14.5 two time points. 

# Abstract
In the mammalian auditory system, frequency discrimination depends on morphological and physiological properties of the organ of Corti that gradually change along the longitudinal (tonotopic) axis of the organ and therefore shape the tuning properties of individual hair cells. At the molecular level, those frequency-specific characteristics are mirrored in gene expression gradients, which require tonotopic patterning of the cochlea. To infer molecular mechanisms, we reconstructed the embryonic cochlear duct in 3D space from single-cell gene expression data and proposed two hypotheses regarding spatial patterning. Analyzing scRNA-seq data from two developmental time points suggested that morphogens, rather than a timing-related mechanism confer spatial identity in the cochlea. Subsequently, retinoic acid and sonic hedgehog pathways were identified to form opposing tonotopic gradients and functional interrogation utilizing cochlear explants suggested that both pathways jointly specify the tonotopic axis during development of the murine organ of Corti.

# Analytical Pipelines
Here we include analytical pipeline code for our manuscript.
* Seurat v3 pipeline: Seurat v3 pipeline is to analyze scRNA-seq datasets from both E12.5 and E14.5 time points. We considered E12.5 and E14.5 datasets separately. For each dataset, we conducted QC steps, identified clusters, annotated cell type for each cluster using canonical marker genes, and determined differentially expressed genes for each cell type. We followed the documentations from https://satijalab.org/seurat/.
* LIGER pipeline: LIGER pipeline is to joint analyze E12.5 and E14.5 scRNA-seq datasets. We co-embedded these two datasets onto the same UMAP and validated the identified clusters from `Seurat v3 pipeline`. We followed the documentation of LIGER from http://htmlpreview.github.io/?https://github.com/welch-lab/liger/blob/master/vignettes/Integrating_multi_scRNA_data.html. 
* CellTrails pipeline: CellTrails pipeline is to determine sub-populations of the cochlear duct. Here, we concentrated on cochlear roof cells and cochlear floor cells, which compose of cochlear apex cells and cochlear base cells. E12.5 and E14.5 datasets were considered separately. To rule out the cell cycle effects, we filtered out the cell cycle genes. For each dataset, we conducted dimension reduction using spectral embedding to identify sub-populations of the cochlea, which results in 10 and 14 sub-populations of the cochlea for E12.5 and E14.5 data, respectively. Furthermore, we aggreated the subpopulations into 6 meta clusters for each time point by conducting hierarchical clustering analysis and annotated the metaclusters based on the differentially expressed genes. Hair cells and supporting cells were removed from our analysis due to their unique transcriptome profiles and limited cell number. This pipeline prepared the datasets and annotations for 2D/3D cochlea reconstruction. We followed the documentation from https://hellerlab.stanford.edu/celltrails/. 
* AUCell pipeline: AUCell is to calculate gene set enrichment score on single-cell resolution. AUCell, a ranking-based method, uses the “Area Under the Curve” (AUC) of the recovery curve to determine the enrichment of GO terms on individual cells. GO terms with at least 4 genes related were subjected for further analysis. E12.5 and E14.5 datasets were considered separately. First, we selected cells annotated as cochlear duct, including cochlear floor and cochlear roof cells, and then ranked all genes for each cell using the function *AUCell_buildRanking* with default settings. Next, AUC for each GO term for each cell was calculated using *AUCell_calcAUC* function, and top 10% of the genes in the ranking were used. To identify the differentially enriched pathways between cochlear apex and cochlear base, we selected the corresponding cells and conducted Wilcoxon sum-rank test with Benjamini-Hochberg (BH) correction (*P*-adjust < 0.05) for E12.5 and E14.5 datasets, respectively. Similarly, we determined differential pathways between cochlear roof and cochlear floor cells. We followed the documentation from https://www.bioconductor.org/packages/devel/bioc/vignettes/AUCell/inst/doc/AUCell.html.


# Additional Analysis

Besides the existing algorithms we applied, we also conducted novel analysis for our manuscript. 
* 2D/3D reconstruction: To resolve the 2D and 3D original anatomy of the cochlea, we reconstructed each cell’s relative position for each time point using scRNA-seq data. We assume that spatial domains of the developing cochlear duct are embedded in a continuum of gene expression, which should allow us to reconstruct the cochlear duct in 3D space. Specifically, we first applied CellTrails to identify subpopulations and further determined the metaclusters for each dataset. Next, we applied PCA by utilizing the differentially expressed genes between metaclusters and the original apex, base, and roof clusters for each time point, respectively. The result was projected onto a 2D scaled PCA plot, where the x-axis is PC2 scaled by PC3 distinguishing the cell identities and the y-axis corresponds to PC1 indicating the tonotopic axis of the cochlea. The 3D anatomy of the cochlear duct was reconstructed by projecting the 2D scaled PCA onto a cylinder surface, whereby the height of the cylinder represents the longitudinal axis. For better visualization, we generated a circular projection by flattening the apex-base axis. The reconstruction is validated by asymmetrically expressed marker genes. In summary, the 3D reconstruction of the cochlear duct faithfully recapitulated the three major axes of the developing organ. The `3D_reconstruction.R` script include the functions related 2D/3D reconstruction of the cochlea. 

* Gradually expressed gene identification: To identify transcripts that potentially reflect tonotopic identity, we extracted gradually expressed genes (GEGs) along the tonotopic axis. First, for each individual gene, we conducted Student t-tests (*P*-value < 0.05) to determine whether the gene is differential between cochlear apex and cochlear base. Next, differentially expressed genes were analyzed to identify gradually expressed genes along the tonotopic axis. Specifically, PCA was performed using all genes as features and PC1 was utilized to approximate each cell’s relative position along the apex-to-base axis. For each individual gene, LOWESS regression was used to determine the relationship between gene expression level and PC1. Also, linear theoretical lines along apex-to-base axis were generated based on the maximum and minimum value of each gene. The regression line as empirical line was compared with the simulated theoretical line using Kullback–Leibler (KL) divergence. To remove genes that were non-gradually expressed along the tonotopic axis, we calculated the expression level difference between the maximum value and the minimum value for each individual gene. Next, we determined the overlapped GEGs between E12.5 and E14.5 time points and classified the GEGs into two categories: genes with decreasing and genes with increasing expression level along the apex-to-base axis. The `GEG_identification.R` script include the functions related to gradually expressed gene identification.

* Spatial Reconstruction of Cochlear Floor Including HCs and SCs: To reconstruct cochlear floor including hair cells (HCs) and supporting cells (SCs) in 2D space, we conducted PCA analysis by leveraging the cochlear floor specific GEGs as features. HCs, SCs, and remaining floor cells were represented in individual columns of the 1D PCA visualizations.

* Similarity method to test two hypotheses about developing cochlea extension: To test two potential mechanisms conferring tonotopic identity, we calculated pairwise cell similarity scores between E12.5 and E14.5 datasets. Specifically, we first generated metacells to denoise the dataset by aggregating the nearest 10 neighbor cells for E12.5 and E14.5 data, respectively. By leveraging the GEGs, we calculated distance for each pair of metacells between E12.5 and E14.5 and convert distance to similarity with various methods. For Euclidean distance matrix, traditional inverse method (1/(1+distance)) or radial basis function (RBF) was used to generate the similarity matrix. To test the robustness of our analysis, various parameters we used within the analysis were systematically tested. The `Similarity.R` script include the functions related to calculate the similarity between cells and test our hypotheses about the cochlear duct extension during development. 




