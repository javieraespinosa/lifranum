
FROM ukwa/webarchive-discovery:warc-discovery-3.1.0 AS discovery

FROM jupyter/base-notebook
USER root

# Java
ENV OPENJDK_VERSION="8"
ENV JAVA_HOME="/usr/lib/jvm/java-${OPENJDK_VERSION}-openjdk-amd64"
RUN apt-get -y update \
 && apt-get install --no-install-recommends -y "openjdk-${OPENJDK_VERSION}-jdk-headless" \
 && apt-get clean  \
 && rm -rf /var/lib/apt/lists/*

# Jupyter default config
RUN echo 'c.NotebookApp.open_browser = False' >> /home/jovyan/.jupyter/jupyter_notebook_config.py \
 && echo 'c.NotebookApp.base_url     = "/"'   >> /home/jovyan/.jupyter/jupyter_notebook_config.py \
 && echo 'c.NotebookApp.ip           = "*"'   >> /home/jovyan/.jupyter/jupyter_notebook_config.py \
 && echo 'c.NotebookApp.token        = ""'    >> /home/jovyan/.jupyter/jupyter_notebook_config.py

# Web-Archive Discovery indexer
COPY --from=discovery /jars/warc-indexer.jar /jars/warc-indexer.jar

# Python modules
RUN pip install xmltodict

# original jupyter/base-notebook config
USER $NB_UID
WORKDIR $HOME


