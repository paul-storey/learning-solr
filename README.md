# tei-solr

## Start Solr

```
./bin/solr start -c -p 8983 -s example/cloud/node1/solr
./bin/solr start -c -p 7574 -s example/cloud/node2/solr -z localhost:9983
```

Then go to [admin UI](http://localhost:8983/solr/#/)

[Solr Tutorial](https://solr.apache.org/guide/8_11/solr-tutorial.html) is a good source of help.
How to create a collection:
```
./bin/solr create -c medieval -s 2 -rf 2
```
To disable data-driven schema functionality:
```
bin/solr config -c medieval -p 7574 -action set-user-property -property update.autoCreateFields -value false
```

## Schema API
To get the whole schema:
```
http://127.0.0.1:8983/solr/medieval/schema
```
or just the fields:
```
http://127.0.0.1:8983/solr/medieval/schema/fields
```
or the fieldtypes
```
http://127.0.0.1:8983/solr/medieval/schema/fieldtypes
```
to add a field:
```
curl -X POST -H 'Content-type:application/json' --data-binary '{"add-field": {"name":"title", "type":"text_general", "multiValued":false, "stored":true}}' http://localhost:8983/solr/medieval/schema
```
boolean field example:
```
curl -X POST -H 'Content-type:application/json' --data-binary '{"add-field": {"name":"is_composite", "type":"boolean", "default":"false", "multiValued":false, "stored":true}}' http://localhost:8983/solr/medieval/schema
```
copy field example
```
curl -X POST -H 'Content-type:application/json' --data-binary '{ "add-copy-field":{ "source":"title", "dest":[ "_text_" ]} }' http://localhost:8983/solr/medieval/schema
```

## How to post docs for indexing
```
./bin/post -c medieval ~/Documents
```


## How to delete a collection:
Collections can be deleted using the admin ui.

Or from the command line: `bin/solr delete -c medieval`

## How to stop solr
```
./bin/solr stop -all
```
## How to control which fields are searched by default:
Refer to https://solr.apache.org/guide/8_11/initparams-in-solrconfig.html

## How to retrieve config
```
http://localhost:8983/solr/medieval/config
```
In the return value it can be seen that the default field to which searches are directed is `_text_` (under `initParams`).

