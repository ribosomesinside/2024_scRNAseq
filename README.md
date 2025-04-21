Project's learning objectives:
1. Quantifying scRNAseq data
Example: used Cellranger within nf-core scRNAseq pipeline to analyse 5 scRNAseq datasets: 96h (2 repeats), 120h (2 repeats), 96h (1 repeat, different study)

3. Learning how to use and troubleshoot nf-core pipelines (parameters, versions)
* fetchngs was not downloading the files correctly due to the number of reads >2, hence needed to use sratools enforsment  
`nextflow -log nextflow.log run nf-core/fetchngs -profile crick -process.executor local -config custom.config --input samplesheet_fetch_scRNAseq.csv --outdir pub_scRNAseq --force_sratools_download -r 1.10.0 -resume`
* cellranger error at the last step of alignment- converting from `.h5` file to seurat due to the unconventional format of one gene name. Solution-> loading `filtered_feature_bc_matrix.h5` directly to R.
3. Parsing and wrangling data formats
   * script to prepare fastq with the conventional fastq names linking to original fastq names
   * from .h5 to seurat, from .seurat to sce
4. Analysis structure:
* having a main project folder on the server, from their fork to **nf-core** and **seurat**
   - nf-core
     * fork to `ngsfetch` and `scrnaseq` where all work is saved respectively (separate folders for each nf-core pipeline)
   - seurat
     * has copies of all *.h5 files obtained in quantification
     * eventually used local computer to work with seurat objects, used R.proj with data, scripts, figures subfolders
5. Reporting (md documents, Renv package)
* Using R.proj for downstream analysis by seurat, md documents, declaring and using parameters in md document
6. QC (filtering, doublets)
  * predicting doublets from raw dataset using `scDblFinder` package
  * filtering barcodes and reads according to gaussian distribution in loupe browser
7. Cell annotation (manual/automated), stratification by a feature expression (in this case-Brk)
  * manual cell annotation according to: 1) analysis from a publication, 2) main cell type features known from literature, 3) dotplot, 4) playing with cluster resolution
  * `FindMarkers` between Brk-expressing cells and the rest
8. Diff expression analysis (dataset integration within cell types/time points, statistical analysis)
  * merging repeats of 96h time point (using `vars.to.regress = "orig.ident"`)
