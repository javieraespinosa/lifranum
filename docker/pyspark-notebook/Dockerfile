
# Based on the pyspark-notebook Dockerfile developped by the Jupyter Development Team
#     https://github.com/jupyter/docker-stacks/blob/master/pyspark-notebook/Dockerfile

FROM jupyter/scipy-notebook

# Fix DL4006
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# ------------------------------------
# Java
# ------------------------------------

ENV OPENJDK_VERSION="11"
ENV JAVA_HOME="/usr/lib/jvm/java-${OPENJDK_VERSION}-openjdk-amd64"

RUN apt-get -y update && apt-get install --no-install-recommends -y \
    "openjdk-${OPENJDK_VERSION}-jre-headless" \
    ca-certificates-java \
 && apt-get clean  \
 && rm -rf /var/lib/apt/lists/*


# ------------------------------------
# Apache Spark 
# ------------------------------------

WORKDIR /tmp

ENV SPARK_VERSION="3.0.0" \
    HADOOP_VERSION="2.7"

# Install curl
RUN apt-get -y update && apt-get install --no-install-recommends -y curl \
 && apt-get clean  \
 && rm -rf /var/lib/apt/lists/*

RUN curl -L "https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" > spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
 && tar xzf "spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" -C /usr/local --owner root --group root --no-same-owner \
 && rm "spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz"

WORKDIR /usr/local

# Configure Spark
ENV SPARK_HOME=/usr/local/spark
ENV SPARK_OPTS="--driver-java-options=-Xms1024M --driver-java-options=-Xmx4096M --driver-java-options=-Dlog4j.logLevel=info" \
    PATH=$PATH:$SPARK_HOME/bin

# Add a link in the before_notebook hook in order to source automatically PYTHONPATH
RUN ln -s "spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}" spark \
 && mkdir -p /usr/local/bin/before-notebook.d  \ 
 && ln -s "${SPARK_HOME}/sbin/spark-config.sh" /usr/local/bin/before-notebook.d/spark-config.sh

# Fix Spark installation for Java 11 and Apache Arrow library
# see: https://github.com/apache/spark/pull/27356, https://spark.apache.org/docs/latest/#downloading
RUN cp -p "$SPARK_HOME/conf/spark-defaults.conf.template" "$SPARK_HOME/conf/spark-defaults.conf" \
 && echo 'spark.driver.extraJavaOptions="-Dio.netty.tryReflectionSetAccessible=true"' >> $SPARK_HOME/conf/spark-defaults.conf \
 && echo 'spark.executor.extraJavaOptions="-Dio.netty.tryReflectionSetAccessible=true"' >> $SPARK_HOME/conf/spark-defaults.conf

USER $NB_UID

# Install pyarrow
RUN conda install --quiet --yes --satisfied-skip-solve 'pyarrow=3.0.*' \
 && conda clean --all -f -y \
 && fix-permissions "${CONDA_DIR}" \
 && fix-permissions "/home/${NB_USER}"

WORKDIR $HOME


#------------------------------------------
# Archives Unleashed Toolkit (AUT)
#------------------------------------------

USER root
WORKDIR /

ENV AUT_VERSION=0.90.0  \
    AUT_HOME=/aut

RUN wget -q https://github.com/archivesunleashed/aut/releases/download/aut-${AUT_VERSION}/aut-${AUT_VERSION}.zip  \
 && wget -q https://github.com/archivesunleashed/aut/releases/download/aut-${AUT_VERSION}/aut-${AUT_VERSION}-fatjar.jar \
 && mkdir ${AUT_HOME} \
 && mv aut-* ${AUT_HOME}

# Add AUT jar and python wrapper to the Spark runtime-environment   
#    https://spark.apache.org/docs/latest/configuration.html#runtime-environment
RUN var1=$( echo AUT_HOME/aut-AUT_VERSION-fatjar.jar | sed "s|AUT_HOME|$AUT_HOME|" | sed "s|AUT_VERSION|$AUT_VERSION|" ) \
 && var2=$( echo AUT_HOME/aut-AUT_VERSION.zip        | sed "s|AUT_HOME|$AUT_HOME|" | sed "s|AUT_VERSION|$AUT_VERSION|" ) \
 && echo spark.driver.extraClassPath=$var1 >> $SPARK_HOME/conf/spark-defaults.conf \
 && echo spark.submit.pyFiles=$var2        >> $SPARK_HOME/conf/spark-defaults.conf 


#------------------------------------------
# Use as Colab local runtime
#     https://research.google.com/colaboratory/local-runtimes.html
#------------------------------------------

RUN pip install jupyter_http_over_ws \
 && jupyter serverextension enable --py jupyter_http_over_ws


#------------------------------------------
# Final Configuration
#------------------------------------------

# Jupyter 
RUN echo 'c.NotebookApp.allow_origin = "https://colab.research.google.com"' >> /home/jovyan/.jupyter/jupyter_notebook_config.py \
 && echo 'c.NotebookApp.port_retries = 0'     >> /home/jovyan/.jupyter/jupyter_notebook_config.py \
 && echo 'c.NotebookApp.open_browser = False' >> /home/jovyan/.jupyter/jupyter_notebook_config.py \
 && echo 'c.NotebookApp.base_url     = "/"'   >> /home/jovyan/.jupyter/jupyter_notebook_config.py

# Python (extra modules)
COPY requirements.txt /requirements.txt
RUN pip install -r /requirements.txt 

USER $NB_UID
WORKDIR $HOME
