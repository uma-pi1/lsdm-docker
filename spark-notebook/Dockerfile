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


# Install Java Kernel
RUN chown -R jovyan /home/jovyan
USER ${NB_UID}
RUN cd /home/jovyan
RUN wget https://github.com/padreati/rapaio-jupyter-kernel/releases/download/1.2.0/rapaio-jupyter-kernel-1.2.0.jar
RUN java -jar rapaio-jupyter-kernel-1.2.0.jar -i -auto

RUN sed -i '4i \
    "--add-exports",\
    "java.base/sun.nio.ch=ALL-UNNAMED",' /home/jovyan/.local/share/jupyter/kernels/rapaio-jupyter-kernel/kernel.json

RUN mkdir /home/jovyan/shared
