# PTCA Image (enabling Singularity Support)
FROM mcr.microsoft.com/aifx/acpt/stable-ubuntu2004-cu117-py38-torch200:{{latest-image-tag:biweekly\.\d{6}\.\d{1}.*}}

RUN apt-get update && apt-get -y upgrade

WORKDIR /
ENV CONDA_PREFIX=/azureml-envs/model-evaluation
ENV CONDA_DEFAULT_ENV=$CONDA_PREFIX

COPY conda.yaml .
# Create conda environment
RUN conda env create -p $CONDA_PREFIX -f conda.yaml -q && \
    rm conda.yaml && \
    conda run -p $CONDA_PREFIX pip cache purge && \
    conda clean -a -y

# Prepend path to AzureML conda environment
ENV PATH=$CONDA_PREFIX/bin:$PATH

# This is needed for mpi to locate libpython
ENV LD_LIBRARY_PATH $CONDA_PREFIX/lib:$LD_LIBRARY_PATH

# not able to install packages with extra requires in conda.yaml
RUN pip install azureml-acft-common-components["image"]>=0.0.9
