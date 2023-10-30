
# Getting Started with Apache Atlas

The objective of this tutorial is to help you get started with [Apache Atlas](http://atlas.apache.org/). Specifically, you will learn how to:

* Run Atlas using docker.
* Explore Atlas **default data types** used for modelling metadata objects.
* Populate Atlas with **types** and **entities** (i.e., types instances) samples.
* Visualize metadata objects using Atlas's WebUI.

## Requirements

* [Docker](https://docs.docker.com/get-docker/) desktop
* Or [Play-with-Docker](http://play-with-docker.com/) (zero installation)

New to Docker? Read [Docker get started](https://docs.docker.com/get-started/overview/) and [Overview of Docker Compose](https://docs.docker.com/compose/).

## Before Starting

Atlas is a java application that requires other systems to run. Namely, HBase, Kafka, Solr (cf. [Atlas architecture](https://atlas.apache.org/#/Architecture)). For simplicity, you will run Atlas in **local mode**. In this mode, Atlas uses HBase, Kafka and Solr embedded versions, avoiding any further configuration.

The [atlas-tutorial1.yml](./atlas-tutorial1.yml) file you will use in this tutorial has the following content:

```yml
version: '3.6'
services:
   atlas:
      image: sburn/apache-atlas:2.1.0
      container_name: atlas
      command: /opt/apache-atlas-2.1.0/bin/atlas_start.py
      ports: 
         - 21000:21000
```

Note the reference to the [sburn/apache-atlas](https://github.com/sburn/docker-apache-atlas) **docker image**. This image contains Atlas's latest version. Containers of this image run by default Atlas in **local mode**.

## Running Atlas

_Play-with-Docker = PWD_

* Open a  terminal (or **add a new instance** to your [PWD](http://play-with-docker.com/) playground).
  
* Clone the [lifranum repository](http://github.com/javieraespinosa/lifranum):

    ```sh
    git clone http://github.com/javieraespinosa/lifranum
    ```

* Move to the `tutorials` folder:

    ```sh
    cd lifranum/tutorials
    ```

* Pull the [sburn/apache-atlas](https://github.com/sburn/docker-apache-atlas) docker image:

    ```sh
    docker-compose -f atlas-tutorial1.yml pull
    ```

* Start the `atlas` container:

    ```sh
    docker-compose -f atlas-tutorial1.yml up atlas 
    ```

* Login into the **Atlas's WebUI** ([localhost:21000](http://localhost:21000/) or the PWD instance's IP) using the login/password: **admin/admin**.

    > The `atlas` container takes **~3 min to start**. During this time the Atlas WebUI is unavailable.

## Atlas Type System

Atlas uses a [type system](https://atlas.apache.org/#/TypeSystem) for metadata modelling (cf. [Atlas architecture](https://atlas.apache.org/#/Architecture)).

The Atlas's type system distinguishes two concepts:

* **Type**. Defines the _schema_ of a particular class of metadata objects and how they are stored and accessed.

* **Entity**. Represents a specific value or _metadata object_ (e.g., an instance of the `Entity` type).

By default, Atlas is preloaded with types modelling metadata from popular technologies (cf. [atlas default models](https://github.com/apache/atlas/tree/master/addons/models)). For instance, the diagram below shows the types modelling **AWS S3 metadata objects**.  (cf. [atlas-s3-types](https://docs.cloudera.com/runtime/7.2.9/atlas-extract-aws/topics/atlas-s3-entities-created-in-atlas.html) for more information).

![ atlas-s3-types-diagram)](https://docs.cloudera.com/runtime/7.2.9/atlas-extract-aws/images/atlas-model-s3.png)

### DYS: Explore Atlas Type System

_DYS= Do it yourself!_

Using the **Apache WebUI**:

1. Explore Atlas default types using the **Search by Type** drop-down list.
  
2. Filter the list by **Groups of Types**. For instance, show only the **Azure types** modelling the _Azure Data Lake Storage_ objects).

## Creating Entities

There are 3 ways of creating an entity in Atlas:

* Via the [Atlas REST API](http://atlas.apache.org/api/v2/ui/index.html#/EntityREST) (synchronous)

* Using [Kafka messages](https://atlas.apache.org/#/Notifications) (asynchronous)
  >
  > * [Atlas Hooks](https://atlas.apache.org/#/HookHBase) are build on top of kafka messages

* Via the Atlas WebUI
    >
    > * Uses the REST API
    > * Can create only entities of type **hdfs_path**

### DYS: Populate your Atlas instance

1. Create a **hdfs_path** entity using the Atlas' WebUI.
   * Explore the UI and identify the _mandatory_ and _optional_ entity attributes.

2. Using a **second terminal** (or **PWD instance**):

   * Run the [quick-start example](https://atlas.apache.org/#/QuickStart) to load **types** and **entities** samples into your Atlas instance (cf. [QuickStartV2.java](https://github.com/apache/atlas/blob/master/webapp/src/main/java/org/apache/atlas/examples/QuickStartV2.java)). Use login/password: **admin/admin** if asked.

        ```sh
        # if running locally
        docker exec -it atlas /opt/apache-atlas-2.1.0/bin/quick_start.py
        
        # if running in PWD. 
        # Do not forget to replace ATLAS_IP with your own Atlas IP instance
        docker run --rm -it     \
            sburn/apache-atlas  \ 
            /opt/apache-atlas-2.1.0/bin/quick_start.py http://ATLAS_IP:21000
        ```

   * Analyze the **script output**
   * Use **Atlas' WebUI** for exploring the new metadata objects.
  
    > If Atlas's WebUI is not responding, refresh the webpage

## Stopping ATLAS

```
# only if running locally
docker-compose -f atlas-tutorial1.yml down

# WARNING: only if you want to free all the space used by docker
docker system prune --volumes
```
