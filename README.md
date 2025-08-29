## 环境配置

我们推荐使用 Conda 来管理项目依赖，以确保环境的一致性。



1.  **使用 Conda 创建环境并安装依赖**
    我们提供了一个 `environment.yml` 文件来快速配置环境。运行以下命令，Conda 会自动创建一个名为 `DynamicSD` 的新环境并安装所有必需的包。
    ```bash
    conda env create -f environment.yml -n DynamicSD
    ```

2.  **激活环境**
    环境创建成功后，使用以下命令激活它：
    ```bash
    conda activate DynamicSD
    ```

## 使用说明

详细的使用方法、API文档、示例和注意事项，请参阅 **`DynamicSD使用说明.pdf`** 文件。
