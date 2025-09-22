

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

## Standards for H&E Images in the SD Process
---
    Pixel Accuracy: ≤ 0.65 µm/pixel

    Pixel Dimensions (X, Y): ≤ 30,000

    Tissue Area Percentage: ≥ 80%

## Usage
---
For detailed usage instructions, API documentation, examples, and important notes, please refer to the **`DynamicSD User Manual.pdf`** document.

