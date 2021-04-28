
# Indexing & Browsing WARC files

## TODO
* README file

## Example

```sh
SOLR_IP=192.168.1.17
SOLR_URL=http://$SOLR_IP:8983/solr/discovery

DOCKER_IMAGE=ukwa/webarchive-discovery:warc-discovery-3.1.0

VOL_WARC=$(PWD)/WARC
VOL_CONF=$(PWD)/conf/webarchive-discovery/warc-indexer.conf
VOL_CMD=$(PWD)/indexer.sh

docker run --rm  \
           -v $VOL_WARC:/WARC \
           -v $VOL_CONF:/warc-indexer.conf \
           -v $VOL_CMD:/indexer.sh \
           -e SOLR_URL=$SOLR_URL \
           $DOCKER_IMAGE /bin/sh -c /indexer.sh
```

