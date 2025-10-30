

## Environment
---
We recommend using Conda to manage project dependencies for ensuring environment consistency.



1.  **Use Conda to create an environment and install the dependencies.**
    We provide an environment.yml file for quick environment setup. Run the following command, and Conda will automatically create a new environment named DynamicSD and install all required packages.
    ```bash
    conda env create -f environment.yml -n DynamicSD
    ```

2.  **Activate the environment using the following command**
    ```bash
    conda activate DynamicSD
    ```
### Quick start
---
- mkref

```shell
wget ftp://ftp.ensembl.org/pub/release-99/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
wget ftp://ftp.ensembl.org/pub/release-99/gtf/homo_sapiens/Homo_sapiens.GRCh38.99.gtf.gz

gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
gunzip Homo_sapiens.GRCh38.99.gtf.gz

DynamicSD mkref \
 --genome_name Homo_sapiens_GRCh38 \
 --fasta Homo_sapiens.GRCh38.dna.primary_assembly.fa \
 --gtf Homo_sapiens.GRCh38.99.gtf
```

- count

```shell
 DynamicSD count \
 --id samplename \
 --inputdir rawdata \
 --whitelist-fastq  2D250806020_C2.fq.gz\
 --he-image HE.tif \
 --probe-set  probeV2_human.csv \
 --gtf Homo_sapiens.GRCh38.99.gtf\
 --transcriptome Homo_sapiens_GRCh38 \
 --cellbin \
 --cores 32
```

## Usage
---
For detailed usage instructions, API documentation, examples, and important notes, please refer to the **`DynamicSD User Manual.pdf`** document.

## Analysis
### Inputs

---

**Input Parameters for DynamicSD count**
|  **Argument**  |  **Brief Description**  |
|  :--------: |  :-----:  |
|    --id   |   Final sample name,required   |
|    --inputdir   |   Raw data path,required   |
|    --whitelist-fastq   |   FastQ sequence containing whitelist,required   |
|  --he-image  |  Single H&E brightfield image  |
|--probe-set|CSV file specifying the probe set used, if any|
|--r2-length|Hard trim the input Read 2 to this length before analysis|
|--gtf|genome annotation file|
|--transcriptome|Path of folder containing transcriptome reference|
|--outputdir|Output file path|
|--cellbin|Perform Cellbin analysis|
|--manual-registration|Perform manual registration|
|--micrometer-pixel|micrometer per pixel|
|--umi-alignment|Manual Alignment File for UMI and HE Images|
|--dapi-alignment|Manual Alignment File for DAPI and HE Images|
|--DAPI|DAPI images for supplementary cell segmentation results|
|--CBposition|position of Cell Barcode(s) on the barcode read(0-base)|
|--UMIposition|position of the UMI on the barcode read, same as CBposition(0-base)|
|--cores|set max cores the pipeline may request at one time|




**FASTQ file naming convention**

To serve as input for DynamicSD, FASTQ files must adhere to standard naming conventions：
`[Sample Name]`_S1_L00`[Lane Number]` _`[Read Type]`_001.fastq.gz Where Read Type is one of:
- `R1`: Read 1
- `R2`: Read 2
  
**DynamicSD Image Recommendations**
 ```
    Pixel Accuracy: ≤ 0.65 µm/pixel

    Pixel Dimensions (X, Y): ≤ 30,000

    Tissue Area Percentage: ≥ 80%
```

**Manual Alignment for DynamicSD**

In most cases, DynamicSD will automatically align microscope images with Umi distribution images. If manual alignment is necessary, choose the option that best fits your experiment.
https://github.com/DynamicBiosystems/DynamicSDAssist

**Cell Segmentation**

DynamicSD supports cell segmentation for both HE and DAPI staining. Typically, we only perform cell segmentation on HE images, controlled by the `--cellbin` parameter. If DAPI-based cell segmentation is required to supplement the results, the `--dapi-alignment` and `--DAPI` parameters should be added for control.

### Outputs
#### Overview of output structure
---
As an example, a `DynamicSD count` analysis will display a message similar to the following after completion:
```
├── 16um
│   ├── bin16_adata.h5ad
│   ├── filtered_feature_bc_matrix
│   ├── filtered_feature_bc_matrix.h5
│   └── spatial
├── 50um
│   ├── bin50_adata.h5ad
│   ├── filtered_feature_bc_matrix
│   ├── filtered_feature_bc_matrix.h5
│   └── spatial
├── 8um
│   ├── bin8_adata.h5ad
│   ├── filtered_feature_bc_matrix
│   ├── filtered_feature_bc_matrix.h5
│   └── spatial
├── bin_summary.csv
├── CellBin
│   ├── filtered_feature_bc_matrix
│   ├── filtered_feature_bc_matrix.h5
│   └── spatial
├── insitufocus
│   ├── Cells.geojson
│   ├── chip_region.tif 
│   ├── dynamicsd.insitufocus
│   └── GeneCoord.csv
├── metrics_summary.csv
├── raw_feature_bc_matrix
│   ├── barcodes.tsv.gz
│   ├── features.tsv.gz
│   └── matrix.mtx.gz
└── web_summary.html
```

#### Gene Expression

When DynamicSD count is run on DynaSpatial data, the outputs have a hierarchical structure to accommodate cell segmentation and various binning levels.

|  **File or Directory Name**  |  **Description**  |
|  :--------: |  :-----:  |
|    8um   |   This is the 8μm bin size folder, containing the `filtered_feature_bc_matrix`, `spatial`, `filtered_feature_bc_matrix.h5`, and `adata.h5ad` files.   |
|    16um   |   This is the 16μm bin size folder, containing the `filtered_feature_bc_matrix`, `spatial`, `filtered_feature_bc_matrix.h5`, and `adata.h5ad` files.   |
|    50um   |   This is the 50μm bin size folder, containing the `filtered_feature_bc_matrix`, `spatial`, `filtered_feature_bc_matrix.h5`, and `adata.h5ad` files.   |
|    bin_summary.csv   |  Metrics Summary for Different Bin Sizes   |
|  CellBin  |  Folder containing segmented outputs. containing the `filtered_feature_bc_matrix`, `spatial` and `filtered_feature_bc_matrix.h5` files |
|insitufocus|The `insitufocus` directory contains the essential input files for the InsituFocus software, which is used to visually inspect and validate the cell segmentation results,containing the `Cells.geojson`, `chip_region.tif `, `dynamicsd.insitufocus`, and `GeneCoord.csv` files.  |
|metrics_summary.csv|Run summary metrics in CSV format|
|raw_feature_bc_matrix|Raw Matrix Results at 1μm Resolution,containing the `barcodes.tsv.gz`, `features.tsv.gz`, and `matrix.mtx.gz` files.|
|web_summary.html|Run summary metrics and plots in HTML format|



















  



