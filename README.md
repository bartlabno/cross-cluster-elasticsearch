# cross-cluster-elasticsearch
Proof of concept and instructions on how to enable cross-clustering search on ElasticSearch


## To create indices (per cluster)
### Krakow cluster
> curl -X PUT "localhost:9201/point-of-interest"
### London cluster
> curl -X PUT "localhost:9203/point-of-interest"

## To check if cross clustering seeds are attached and healthy:
curl -XGET -H 'Content-Type: application/json' localhost:9201/_remote/info?pretty

## To put data (per cluster)
### Krakow cluster
> curl -s -H "Content-Type: application/x-ndjson" -XPOST localhost:9201/_bulk --data-binary "@krakow.json"; echo
### London cluster
> curl -s -H "Content-Type: application/x-ndjson" -XPOST localhost:9203/_bulk --data-binary "@london.json"; echo

## Kibana
As this is totally proof of concept and no specific data has been provided so there is a lack of specific information provided within logs. That makes there are not visible correctly on kibana search - because there is no timedate provided. That means it is not visible by default. Anyway as Kibana is using kra-1 node as search node which is using cross-cluster functionality, so cross-clustering is available by default and there is no any additional action needed before searching. To proof cross-clustering is working on Kibana follow it:

* Go to http://localhost:5601 once docker started all containers
* Go to "Discover" tab
* On filter search change "*" into "*:*" and tap Enter
* Scroll down to see results

## Looking for a results

There are many ways to look after results in indices and clusters. Remember you can use wildcards, specific names and commas to be more specific on results which gives you complete freedom on result searching. In fact, you can search on different indices across different clusters, you can use wildcards on multiple similar clusters or indices, you can add part of the name or be more specific. You can also mix multiple clusters using commas to separate them. There is complete freedom on search. This is subject to both - ElasticSearch and later Kibana

> London:point-of-interest
> *:*
> Krakow:point-of-*
> *:point-of-interest
> Krakow:point-of-interest,London:point-of-interest
> London*:*
