# Use an official conda base image with specific version
FROM continuumio/miniconda3:23.10.0-1

# Add metadata
LABEL maintainer="Vito Butardo Jr <vbutardo@swin.edu.au>"
LABEL organization="Swinburne University of Technology"
LABEL department="Department of Chemistry and Biotechnology"
LABEL laboratory="Bioactives in Rice Accessions for Nutriomics (BRAN) Research"
LABEL version="1.0"
LABEL description="Docker environment for MetaboAnalystR with Jupyter integration"
LABEL github="https://github.com/vbutardo/docker-MetaboAnalystR"

# Arguments for version control
ARG R_VERSION=4.3.3
ARG PYTHON_VERSION=3.9
ARG BIOCONDUCTOR_VERSION=3.18

# Set environment variables
ENV CONDA_DIR=/opt/conda \
    LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    R_LIBS_USER=/app/.R/library \
    JUPYTER_ENABLE_LAB=yes \
    PATH="/app/.local/bin:${PATH}"

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    git \
    libcurl4-gnutls-dev \
    libfontconfig1-dev \
    libfreetype6-dev \
    libfribidi-dev \
    libgit2-dev \
    libglpk-dev \
    libgmp3-dev \
    libgsl0-dev \
    libharfbuzz-dev \
    libjpeg-dev \
    libpng-dev \
    libssl-dev \
    libtiff5-dev \
    libxml2-dev \
    wget \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Copy environment files
COPY environment/metaboanalystr_minimal.yml ./environment/
COPY environment/r_packages_list.csv ./environment/
COPY tests/validate_environment.R ./tests/
COPY docs/ ./docs/

# Create conda environment
RUN conda env create -f environment/metaboanalystr_minimal.yml && \
    conda clean -afy

# Set up R environment
SHELL ["conda", "run", "-n", "metaboanalystr", "/bin/bash", "-c"]
RUN R -e 'install.packages(c("BiocManager", "devtools", "remotes"), repos="http://cran.rstudio.com/", dependencies=TRUE)'

# Install R packages
RUN R -e '\
    packages <- read.csv("environment/r_packages_list.csv", stringsAsFactors = FALSE)$Package; \
    bioc_packages <- packages[grepl("^Bioc", packages)]; \
    cran_packages <- packages[!grepl("^Bioc", packages)]; \
    base_packages <- c("base", "compiler", "datasets", "graphics", "grDevices", "grid", \
                      "methods", "parallel", "splines", "stats", "stats4", "tcltk", "tools", "utils"); \
    cran_packages <- cran_packages[!cran_packages %in% base_packages]; \
    if (length(bioc_packages) > 0) BiocManager::install(bioc_packages, update = FALSE, ask = FALSE); \
    if (length(cran_packages) > 0) install.packages(cran_packages, repos = "http://cran.rstudio.com/", dependencies = TRUE)'

# Install MetaboAnalystR
RUN R -e 'BiocManager::install("MetaboAnalystR"); \
    IRkernel::installspec(name = "metaboanalystr", displayname = "R (MetaboAnalystR)")'

# Set up directory structure
RUN mkdir -p \
    /app/data/processed \
    /app/data/temp \
    /app/plots \
    /app/results \
    /app/notebooks \
    /app/environment

# Install Jupyter extensions
RUN conda install -n metaboanalystr -c conda-forge \
    jupyterlab-git \
    jupyter-resource-usage \
    nbdime \
    jupytext \
    && conda clean -afy

# Create non-root user
RUN useradd -m -s /bin/bash analyst && \
    chown -R analyst:analyst /app

# Copy notebooks and set permissions
COPY notebooks/*.ipynb /app/notebooks/
RUN chown -R analyst:analyst /app/notebooks

# Run validation
RUN conda run -n metaboanalystr Rscript tests/validate_environment.R

# Switch to non-root user
USER analyst

# Configure Jupyter
RUN mkdir -p ~/.jupyter && \
    echo "c.NotebookApp.token = ''" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.password = ''" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "conda activate metaboanalystr" >> ~/.bashrc

# Create entrypoint script
RUN echo '#!/bin/bash\n\
conda activate metaboanalystr\n\
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root --notebook-dir=/app/work' > /app/entrypoint.sh && \
    chmod +x /app/entrypoint.sh

# Expose port and set volume
EXPOSE 8888
VOLUME ["/app/work"]

# Health check
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:8888/api || exit 1

# Set entrypoint
ENTRYPOINT ["/app/entrypoint.sh"]