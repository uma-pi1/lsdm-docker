## Install Kernel

```sh
wget https://github.com/padreati/rapaio-jupyter-kernel/releases/download/1.2.0/rapaio-jupyter-kernel-1.2.0.jar
java -jar rapaio-jupyter-kernel-1.2.0.jar -i -auto
```


## Update Kernel Config

Update file
`/home/jovyan/.local/share/jupyter/kernels/rapaio-jupyter-kernel/kernel.json`:

```json
[
  "java",
  "--add-exports",
  "java.base/sun.nio.ch=ALL-UNNAMED",
  "<remaining-lines-below>"
]
```

## Import Spark and Dependencies

In every Java notebook, insert cell with the following content and execute:

```ipython
%%jars
/usr/local/spark/jars
```
