FROM quay.io/jupyter/pyspark-notebook:java-17.0.12

RUN pip install --no-cache-dir 'apache-beam[gcp]' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

RUN pip install --no-cache-dir mysql-connector-python pymongo

USER root

RUN apt-get update
RUN apt-get -y install vim

RUN pip install --upgrade spylon-kernel
RUN python -m spylon_kernel install

USER ${NB_UID}
