
# Jupyter Notebook with AUT toolkit

Jupyter Notebook configured with the [AUT toolkit](https://aut.docs.archivesunleashed.org). This notebook is a custom version of the `pyspark-notebook` docker stack developped by the Jupyter team (cf. [Jupyter docker stacks](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#image-relationships)).

## Features

* AUT toolkit ready
* Compatible with Google Colab

## Usage

Start jupyter notebook using docker-compose:

```sh
docker-compose up
```

Visit http://localhost:8888


## Configuration

The following parameters are configured in the notebook by default (cf. `Dockerfile`):

**Spark** ([spark-defaults.conf](https://github.com/apache/spark/blob/master/conf/spark-defaults.conf.template))

```sh
spark.driver.extraClassPath=/aut/aut-$AUT_VERSION-fatjar.jar
spark.submit.pyFiles=/aut/aut-$AUT_VERSION.zip
spark.driver.extraJavaOptions="-Dio.netty.tryReflectionSetAccessible=true"
spark.executor.extraJavaOptions="-Dio.netty.tryReflectionSetAccessible=true"
```

**Jupyter** ([jupyter_notebook_config.py](https://gist.github.com/IvanMalison/369dbc15a05a41d3ab6616e12d591a94))

```py
c.NotebookApp.allow_origin = "https://colab.research.google.com"
c.NotebookApp.port_retries = 0
c.NotebookApp.open_browser = False
c.NotebookApp.base_url     = "/"
```

## Image building

Modify `requirements.txt` for updating the list of python packages available in the image.

```sh
docker-compose build --force-rm
```

## Use as Colab Local Runtime

1. Go to [Google Colab](https://colab.research.google.com).
2. Open a notebook.
3. Open the connection options and select "_Connect to local runtime_" (cf. image below).
4. Connect to Jypyter local instance: `http://localhost:8888/` (Jupyter listens on port `8888` by default).

![Select Colab Local Runtime](local-runtime.png)

## TODO

* Verify compatibility with Jupyterhub