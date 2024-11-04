# docker-MetaboAnalystR

A Docker implementation of [MetaboAnalystR](https://github.com/xia-lab/MetaboAnalystR) with Jupyter integration for reproducible metabolomics analysis.

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

This repository provides a Docker-based solution implementing FAIR (Findable, Accessible, Interoperable, and Reusable) principles:

#### Findable
- ✅ Version-controlled environment
- ✅ Comprehensive documentation
- ✅ Clear package dependencies
- ✅ Trackable container versions

#### Accessible
- ✅ Docker-based deployment
- ✅ Cross-platform compatibility
- ✅ Simplified installation
- ✅ Pre-configured environment

#### Interoperable
- ✅ Jupyter notebook integration
- ✅ Standardized workflows
- ✅ Consistent environment
- ✅ Platform independence

#### Reusable
- ✅ Reproducible analysis environment
- ✅ Example workflows
- ✅ Validation procedures
- ✅ Version-locked dependencies

## Repository Organization

This repository is a fork of the original [MetaboAnalystR](https://github.com/xia-lab/MetaboAnalystR) organized as follows:

```
docker-MetaboAnalystR/
├── data/               # Data and results
│   ├── processed/     # Processed data files
│   └── temp/         # Temporary files
├── docker/           # Docker configuration
│   ├── Dockerfile
│   └── README_docker.md
├── docs/             # Documentation
│   ├── installation.md
│   └── troubleshooting.md
├── environment/      # Environment configuration
│   ├── metaboanalystr_environment.yml
│   ├── metaboanalystr_minimal.yml
│   └── r_packages_list.csv
├── notebooks/        # Jupyter notebooks
│   ├── 00_setup_and_validation.ipynb
│   └── 01_metabolomics_workflow.ipynb
├── tests/           # Testing
│   ├── docker-tests/
│   └── metaboanalystr-tests/
└── [original MetaboAnalystR files]
```

### Branch Management
- **main**: Synced with upstream MetaboAnalystR repository
- **docker**: Contains Docker implementation and Jupyter integration

### Version Compatibility

| Component | Version |
|-----------|---------|
| MetaboAnalystR | Tracks main branch |
| R | 4.3.3 |
| Bioconductor | 3.18 |
| Python | 3.9 |
| Jupyter | Latest |

## Quick Start

### Prerequisites
- Docker installed
- 8GB+ RAM recommended
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

# Build the image
docker build -f docker/Dockerfile -t docker-metaboanalystr .

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

## Documentation

- [Installation Guide](docs/installation.md)
- [Troubleshooting Guide](docs/troubleshooting.md)
- [Docker Implementation](docker/README_docker.md)
- [Example Workflows](notebooks/)

## Contributing

We welcome contributions! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a Pull Request

## Support

If you encounter issues:
1. Check `docs/troubleshooting.md`
2. Review container logs
3. Submit an issue at https://github.com/vbutardo/docker-MetaboAnalystR/issues

## License

This Docker implementation is released under the MIT License. The original MetaboAnalystR package maintains its own license.

## Contact

- **Developer**: Dr. Vito Butardo Jr.
- **Department**: Department of Chemistry and Biotechnology
- **Laboratory**: Bioactives in Rice Accessions for Nutriomics (BRAN) Research
- **Institution**: Swinburne University of Technology
- **GitHub**: [@vbutardo](https://github.com/vbutardo)

## Acknowledgments

This project is developed at the BRAN Research Laboratory, Department of Chemistry and Biotechnology, Swinburne University of Technology. Special thanks to:
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