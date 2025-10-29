

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

## Standards for H&E Images in the SD Process
---
    Pixel Accuracy: ≤ 0.65 µm/pixel

    Pixel Dimensions (X, Y): ≤ 30,000

    Tissue Area Percentage: ≥ 80%

## Usage
---
For detailed usage instructions, API documentation, examples, and important notes, please refer to the **`DynamicSD User Manual.pdf`** document.

**Specifying Input FASTQs to DynamicSD count**
|  **Argument**  |  **Brief Description**  |
|  :--------: |  :-----:  |
|    --id   |   Final sample name,required   |
|    --inputdir   |   Raw data path,required   |
|    --whitelist-fastq   |   FastQ sequence containing whitelist,required   |

**FASTQ file naming convention**

To serve as input for DynamicSD, FASTQ files must adhere to standard naming conventions：
[Sample Name]_S1_L00[Lane Number] _[Read Type]_001.fastq.gz Where Read Type is one of:
- `R1`: Read 1
- `R2`: Read 2
