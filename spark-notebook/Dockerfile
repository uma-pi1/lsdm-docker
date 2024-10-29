FROM quay.io/jupyter/pyspark-notebook@sha256:999213c55c952aa484128cd0297746703323d4fdfa70fbf66c0922937b3ad45b

# equivalent to:
#
# FROM quay.io/jupyter/pyspark-notebook:spark-3.5.3
#
# on Oct 21, 2024

# Install system-wide packages
USER root
RUN apt-get update -qq 
RUN apt-get -y install -qq vim netcat-traditional

# Install user packages
USER jovyan
WORKDIR /home/jovyan

RUN pip install -q --no-cache-dir \
	'apache-beam[gcp]' \
	mysql-connector-python \
	pyarrow \
	pandas \
	pymongo spylon-kernel

RUN fix-permissions "${CONDA_DIR}" && fix-permissions "/home/jovyan"

# Install Scala kernel
RUN python -m spylon_kernel install --user

# Install Java kernel
WORKDIR /home/jovyan/local
RUN wget https://github.com/padreati/rapaio-jupyter-kernel/releases/download/1.2.0/rapaio-jupyter-kernel-1.2.0.jar
RUN java -jar rapaio-jupyter-kernel-1.2.0.jar -i -auto

RUN sed -i '4i \
    "--add-exports",\
    "java.base/sun.nio.ch=ALL-UNNAMED",' /home/jovyan/.local/share/jupyter/kernels/rapaio-jupyter-kernel/kernel.json

# Setup shared directory
RUN mkdir /home/jovyan/shared

# Start Jupyterlab in home directory
WORKDIR /home/jovyan
