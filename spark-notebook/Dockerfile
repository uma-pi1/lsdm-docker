FROM jupyter/pyspark-notebook:java-11.0.15

RUN pip install --no-cache-dir 'apache-beam[gcp]' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

RUN pip install --no-cache-dir mysql-connector-python pymongo

USER root

RUN apt-get update
RUN apt-get -y install vim

RUN pip install --upgrade spylon-kernel
RUN python -m spylon_kernel install


RUN wget https://github.com/SpencerPark/IJava/releases/download/v1.3.0/ijava-1.3.0.zip
RUN unzip ijava-1.3.0.zip -d ijava
RUN cd ijava; python install.py --sys-prefix
RUN cd ..


USER ${NB_UID}
