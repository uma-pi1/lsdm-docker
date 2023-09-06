FROM jupyter/pyspark-notebook

RUN pip install --no-cache-dir 'apache-beam[gcp]' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER root

RUN apt-get update
RUN apt-get -y install vim

USER ${NB_UID}
