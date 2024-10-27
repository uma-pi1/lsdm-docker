FROM quay.io/jupyter/pyspark-notebook:java-17.0.12

USER root
RUN apt-get update
RUN apt-get -y install vim

# switch to user
RUN chown -R jovyan /home/jovyan
USER jovyan
WORKDIR /home/jovyan

# install packages
RUN pip install --no-cache-dir 'apache-beam[gcp]' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

RUN pip install --no-cache-dir mysql-connector-python pymongo

RUN pip install --no-cache-dir pyarrow pandas

# install kernels

# RUN pip install --upgrade spylon
# RUN pip install --upgrade spylon-kernel
# Temporary fixes until spylon/spylon-kernel are updated
RUN mkdir /home/jovyan/local
WORKDIR /home/jovyan/local
RUN git clone https://github.com/vericast/spylon.git spylon
RUN git clone https://github.com/vericast/spylon-kernel.git spylon-kernel
WORKDIR /home/jovyan/local/spylon
RUN git fetch origin pull/57/head:pull_57
RUN git checkout pull_57
RUN pip install -e .
WORKDIR /home/jovyan/local/spylon-kernel
RUN git fetch origin pull/71/head:pull_71
RUN git checkout pull_71
RUN pip install -e .

WORKDIR /home/jovyan/local
RUN wget https://github.com/padreati/rapaio-jupyter-kernel/releases/download/1.2.0/rapaio-jupyter-kernel-1.2.0.jar
RUN java -jar rapaio-jupyter-kernel-1.2.0.jar -i -auto

RUN sed -i '4i \
    "--add-exports",\
    "java.base/sun.nio.ch=ALL-UNNAMED",' /home/jovyan/.local/share/jupyter/kernels/rapaio-jupyter-kernel/kernel.json

# setup shared directory
RUN mkdir /home/jovyan/shared

# start Jupyterlab in home directory
WORKDIR /home/jovyan

