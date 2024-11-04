# docker-MetaboAnalystR

A Docker implementation of [MetaboAnalystR](https://github.com/xia-lab/MetaboAnalystR) with Jupyter integration for reproducible metabolomics analysis.

## Repository Organization

This repository is a fork of the original [MetaboAnalystR](https://github.com/xia-lab/MetaboAnalystR) organized as follows:

```
docker-MetaboAnalystR/
├── main (tracks upstream MetaboAnalystR)
└── docker (contains Docker implementation)
```

### Branch Management
- **main**: Synced with upstream MetaboAnalystR repository
- **docker**: Contains Docker implementation and Jupyter integration

### Staying Updated
```bash
# Add upstream remote
git remote add upstream https://github.com/xia-lab/MetaboAnalystR.git

# Fetch upstream changes
git fetch upstream

# Update main branch
git checkout main
git merge upstream/main

# Update docker branch
git checkout docker
git merge main
```

## Introduction

MetaboAnalystR, developed by the [Xia Lab](https://github.com/xia-lab/MetaboAnalystR), is a comprehensive R package for metabolomics data analysis and interpretation. While the original package provides powerful analytical capabilities, users often encounter installation challenges. This Docker implementation addresses these challenges through FAIR (Findable, Accessible, Interoperable, and Reusable) principles:

### FAIR Implementation
- **Findable**: 
  - Version-controlled environment
  - Comprehensive documentation
  - Clear package dependencies
  - Trackable container versions

- **Accessible**:
  - Docker-based deployment
  - Cross-platform compatibility
  - Simplified installation
  - Pre-configured environment

- **Interoperable**:
  - Jupyter notebook integration
  - Standardized workflows
  - Consistent environment
  - Platform independence

- **Reusable**:
  - Reproducible analysis environment
  - Example workflows
  - Validation procedures
  - Version-locked dependencies

## Project Structure
```
docker-MetaboAnalystR/
├── data/
│   ├── processed/
│   │   ├── normalized_data.csv
│   │   ├── processing_log.txt
│   │   └── raw_dataview.csv
│   ├── temp/
│   │   ├── complete_norm.qs
│   │   ├── data_orig.qs
│   │   └── [other temporary files]
│   └── test_metabolites.csv
├── docker/
│   └── Dockerfile
├── docs/
│   ├── installation.md
│   └── troubleshooting.md
├── environment/
│   ├── metaboanalystr_environment.yml
│   ├── metaboanalystr_minimal.yml
│   └── r_packages_list.csv
├── notebooks/
│   ├── 00_setup_and_validation.ipynb
│   └── 01_metabolomics_workflow.ipynb
├── plots/
│   └── pca_scores_2ddpi72.png
├── results/
│   ├── analysis_summary.txt
│   └── session_info.txt
└── tests/
    ├── test_workflow.ipynb
    └── validate_environment.R
```

## Installation

### Prerequisites
- Docker installed on your system
- At least 8GB RAM available
- 20GB free disk space

### Option 1: Using Pre-built Image
```bash
# Pull the image
docker pull vbutardo/docker-metaboanalystr:latest

# Run the container
docker run -d \
  --name metaboanalystr \
  -p 8888:8888 \
  -v $(pwd):/app/work \
  vbutardo/docker-metaboanalystr:latest
```

### Option 2: Building from Source
```bash
# Clone the repository
git clone https://github.com/vbutardo/docker-MetaboAnalystR.git
cd docker-MetaboAnalystR

# Switch to docker branch
git checkout docker

# Build the image
docker build -t docker-metaboanalystr .

# Run the container
docker run -d \
  --name metaboanalystr \
  -p 8888:8888 \
  -v $(pwd):/app/work \
  docker-metaboanalystr
```

## Getting Started

1. Access Jupyter Lab at `http://localhost:8888`
2. Navigate to `notebooks/00_setup_and_validation.ipynb`
3. Follow the setup and validation workflow
4. Proceed to `01_metabolomics_workflow.ipynb` for analysis

## Version Compatibility

| Component | Version |
|-----------|---------|
| MetaboAnalystR | Tracks main branch |
| R | 4.3.3 |
| Bioconductor | 3.18 |
| Python | 3.9 |
| Jupyter | Latest |

## Troubleshooting

### Common Issues

1. Missing directories
```bash
mkdir -p data/processed data/temp plots results
```

2. Permission issues
```bash
docker exec metaboanalystr chown -R analyst:analyst /app/work
```

3. Memory issues
```bash
docker run -d \
  --name metaboanalystr \
  -p 8888:8888 \
  -v $(pwd):/app/work \
  -m 12g \
  docker-metaboanalystr
```

## Validation

1. Run `00_setup_and_validation.ipynb`
2. Check `results/environment_summary.txt`
3. Verify package installation
4. Run `tests/test_workflow.ipynb`

## Support

1. Check `docs/troubleshooting.md`
2. Review container logs
3. Submit an issue at https://github.com/vbutardo/docker-MetaboAnalystR/issues

## Contributing

We welcome contributions! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a Pull Request

## License

This Docker implementation is released under the MIT License. The original MetaboAnalystR package maintains its own license.

## Contact

- **Developer**: Dr. Vito Butardo Jr.
- **Department**: Department of Chemistry and Biotechnology
- **Laboratory**: Bioactives in Rice Accessions for Nutriomics (BRAN) Research
- **Institution**: Swinburne University of Technology
- **GitHub**: [@vbutardo](https://github.com/vbutardo)

## Acknowledgments

This project is developed at the TeamBRAN Research Laboratory, Department of Chemistry and Biotechnology, Swinburne University of Technology. Special thanks to:
- The [Xia Lab](https://github.com/xia-lab/MetaboAnalystR) for developing MetaboAnalystR
- The MetaboAnalyst team for their metabolomics platform
- The Bioconda community for package management solutions
- The Jupyter team for their interactive notebook environment

## Citations

Please cite both the original MetaboAnalystR package and this Docker implementation:

```bibtex
@software{metaboanalystr_2024,
  author = {Xia Lab},
  title = {MetaboAnalystR},
  year = {2024},
  publisher = {GitHub},
  url = {https://github.com/xia-lab/MetaboAnalystR}
}

@software{docker_metaboanalystr_2024,
  author = {Butardo, Vito Jr},
  affiliation = {Department of Chemistry and Biotechnology, Swinburne University of Technology},
  laboratory = {Bioactives in Rice Accessions for Nutriomics (BRAN) Research},
  title = {docker-MetaboAnalystR},
  year = {2024},
  publisher = {GitHub},
  url = {https://github.com/vbutardo/docker-MetaboAnalystR}
}
```