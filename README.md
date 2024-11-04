# MetaboAnalystR Jupyter Environment

A comprehensive environment setup for running MetaboAnalystR in Jupyter notebooks, providing a reproducible and validated solution for metabolomics data analysis.

## Why This Repository?

### Current MetaboAnalystR Installation Challenges

The standard MetaboAnalystR installation process involves:
- Manual R package installation
- Multiple dependency conflicts
- System-specific configurations
- No standardized environment
- Limited validation options
- Console-only interface

Common issues reported by users:
1. Missing dependencies
2. R version conflicts
3. System library incompatibilities
4. Installation failures
5. Platform-specific problems

### Our Solution

This repository provides:
- ✅ Complete, tested Conda environment
- ✅ Jupyter notebook integration
- ✅ Validated package dependencies
- ✅ Platform-independent setup
- ✅ Comprehensive testing suite
- ✅ Organized workspace structure

## Features

### 1. Reproducible Environment
- R 4.4.1 with over 300 validated packages
- Python 3.9 for Jupyter integration
- Bioconductor 3.18 compatibility
- All dependencies version-locked

### 2. Jupyter Integration
- Custom R kernel for MetaboAnalystR
- Interactive visualization support
- Notebook-based workflows
- Enhanced documentation capabilities

### 3. Validation Suite
- Environment verification tests
- Package compatibility checks
- Functionality validation
- Example workflows

### 4. Directory Structure
```
metaboanalystr-jupyter/
├── environment/    # Environment configuration files
│   ├── metaboanalystr_environment.yml
│   ├── metaboanalystr_minimal.yml
│   └── r_packages_list.csv
├── tests/         # Validation tests
│   ├── validate_environment.R
│   └── test_workflow.ipynb
└── docs/          # Documentation
    ├── installation.md
    └── troubleshooting.md
```

## Quick Start

### Prerequisites
- Anaconda or Miniconda installed
- Git installed
- 8GB+ RAM recommended

### Installation Steps

1. Clone the repository:
```bash
git clone https://github.com/yourusername/metaboanalystr-jupyter.git
cd metaboanalystr-jupyter
```

2. Create conda environment:
```bash
conda env create -f environment/metaboanalystr_minimal.yml
conda activate metaboanalystr
```

3. Verify installation:
```bash
Rscript tests/validate_environment.R
```

4. Start Jupyter:
```bash
jupyter notebook
```

### Example Workflow

```R
# In Jupyter notebook
library(MetaboAnalystR)

# Initialize workspace
mSet <- InitDataObjects("pktable", "stat", FALSE)

# Load and process data
mSet <- Read.TextData(mSet, "data/test_metabolites.csv", "rowu", "disc")
```

## Comparison with Standard Installation

Feature | This Solution | Standard Installation
---|---|---
Environment Reproducibility | ✅ Complete environment capture | ❌ Manual package installation
Platform Independence | ✅ Conda-based isolation | ❌ System-dependent
Jupyter Integration | ✅ Interactive notebooks | ❌ Console only
Validation Suite | ✅ Comprehensive tests | ❌ Basic functionality
Workspace Organization | ✅ Structured directories | ❌ No structure
Documentation | ✅ Complete guides | ❌ Basic README

## Documentation

- [Detailed Installation Guide](docs/installation.md)
- [Troubleshooting Guide](docs/troubleshooting.md)
- [Example Workflows](docs/workflows.md)
- [Development Guide](docs/development.md)

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

## Support

If you encounter any issues:
1. Check the [Troubleshooting Guide](docs/troubleshooting.md)
2. Search existing [Issues](https://github.com/yourusername/metaboanalystr-jupyter/issues)
3. Create a new Issue with details about your problem

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Citation

If you use this environment in your research, please cite:
```
@software{metaboanalystr_jupyter_2024,
  author = {Vito Butardo Jr},
  title = {MetaboAnalystR Jupyter Environment},
  year = {2024},
  publisher = {GitHub},
  url = {https://github.com/yourusername/metaboanalystr-jupyter}
}
```