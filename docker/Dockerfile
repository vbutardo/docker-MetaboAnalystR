# Use an official conda base image
FROM continuumio/miniconda3:latest

LABEL maintainer="Swinburne University of Technology"
LABEL version="1.0"
LABEL description="Comprehensive Docker environment for MetaboAnalystR with Jupyter integration"

# Set working directory
WORKDIR /app

# Copy all necessary files
COPY environment/metaboanalystr_minimal.yml ./environment/
COPY environment/r_packages_list.csv ./environment/
COPY tests/validate_environment.R ./tests/
COPY docs/ ./docs/

# Install system dependencies required for R and compilation
RUN apt-get update && apt-get install -y \
    build-essential \
    libcurl4-gnutls-dev \
    libxml2-dev \
    libssl-dev \
    libgsl0-dev \
    libharfbuzz-dev \
    libfribidi-dev \
    libfreetype6-dev \
    libpng-dev \
    libtiff5-dev \
    libjpeg-dev \
    libgit2-dev \
    libglpk-dev \
    libgmp3-dev \
    libfontconfig1-dev \
    git \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Create conda environment
RUN conda env create -f environment/metaboanalystr_minimal.yml

# Make RUN commands use the conda environment
SHELL ["conda", "run", "-n", "metaboanalystr", "/bin/bash", "-c"]

# Install BiocManager and essential packages first
RUN R -e 'install.packages(c("BiocManager", "devtools", "remotes"), repos="http://cran.rstudio.com/")'

# Install packages from r_packages_list.csv
RUN R -e '\
    packages <- read.csv("environment/r_packages_list.csv", stringsAsFactors = FALSE)$Package; \
    bioc_packages <- packages[grepl("^Bioc", packages)]; \
    cran_packages <- packages[!grepl("^Bioc", packages)]; \
    base_packages <- c("base", "compiler", "datasets", "graphics", "grDevices", "grid", "methods", "parallel", "splines", "stats", "stats4", "tcltk", "tools", "utils"); \
    cran_packages <- cran_packages[!cran_packages %in% base_packages]; \
    if (length(bioc_packages) > 0) BiocManager::install(bioc_packages, update = FALSE, ask = FALSE); \
    if (length(cran_packages) > 0) install.packages(cran_packages, repos = "http://cran.rstudio.com/", dependencies = TRUE)'

# Install MetaboAnalystR and setup Jupyter kernel
RUN R -e '\
    BiocManager::install("MetaboAnalystR"); \
    IRkernel::installspec(name = "metaboanalystr", displayname = "R (MetaboAnalystR)")'

# Set up project directory structure
RUN mkdir -p \
    /app/data/processed \
    /app/data/temp \
    /app/plots \
    /app/results \
    /app/notebooks \
    /app/environment

# Install Jupyter Lab extensions for R integration
RUN conda install -n metaboanalystr -c conda-forge \
    jupyterlab-git \
    jupyter-resource-usage \
    nbdime \
    jupytext

# Create a non-root user
RUN useradd -m -s /bin/bash analyst
RUN chown -R analyst:analyst /app

# Copy example notebooks
COPY notebooks/*.ipynb /app/notebooks/

# Run validation tests
RUN conda run -n metaboanalystr Rscript tests/validate_environment.R

# Switch to non-root user
USER analyst

# Set environment variables
ENV R_LIBS_USER=/app/.R/library
ENV PATH="/app/.local/bin:${PATH}"
ENV JUPYTER_ENABLE_LAB=yes

# Activate conda environment by default
RUN echo "conda activate metaboanalystr" >> ~/.bashrc

# Create jupyter config
RUN mkdir -p ~/.jupyter && \
    echo "c.NotebookApp.token = ''" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.password = ''" >> ~/.jupyter/jupyter_notebook_config.py

# Expose Jupyter port
EXPOSE 8888

# Volume for persistent storage
VOLUME ["/app/work"]

# Health check
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:8888/api || exit 1

# Create entrypoint script
RUN echo '#!/bin/bash\n\
conda activate metaboanalystr\n\
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root --notebook-dir=/app/work' > /app/entrypoint.sh && \
    chmod +x /app/entrypoint.sh

# Start Jupyter Lab
ENTRYPOINT ["/app/entrypoint.sh"]