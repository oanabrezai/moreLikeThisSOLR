# moreLikeThisSOLR
Commands used for Voxxed Days Romania 2021

## Upload Template in Zookeeper
/home/oana/solr-8.9.0_voxxed/bin/solr zk upconfig -z 127.0.0.1:9983 -n _movies_content_ -d [PATH]/cores/_movies_content_/

## Create collection
```
http://localhost:8983/solr/admin/collections?action=CREATE&name=movie_content&numShards=1&replicationFactor=1&collection.configName=_movies_content_
```

## Allow Remote Streaming (not recommended on PROD!!!)
```
curl -H 'Content-type:application/json' -d '{"set-property": {"requestDispatcher.requestParsers.enableRemoteStreaming": true}, "set-property": {"requestDispatcher.requestParsers.enableStreamBody": true}}' http://localhost:8983/solr/movie_content/config
```

## Populate Collection
```
http://localhost:8983/solr/movie_content/update/csv?commit=true&stream.file=[PATH]/IMDb_movies.csv&stream.contentType=text/plain;charset=utf-8&separator=,&f.genre.split=true&f.genre.separator=,
```

## Delete Content
```
http://localhost:8983/solr/movie_content/update?stream.body=%3Cdelete%3E%3Cquery%3E*:*%3C/query%3E%3C/delete%3E&commit=true
```

## MLT without boosting
```
http://localhost:8983/solr/movie_content/select?mlt.fl=original_title,description,genre,avg_vote&mlt.mintf=1&mlt=true&q=imdb_title_id:tt0120737&fl=original_title,score,year&mlt.count=5
```


## MLT with boosting
```
http://localhost:8983/solr/movie_content/select?mlt.fl=original_title,description,genre,avg_vote&mlt.mintf=1&mlt=true&q=imdb_title_id:tt0120737&fl=original_title,score,year&mlt.count=5&mlt.boost=true&mlt.qf=avg_vote^40 genre^30  original_title^20 description
```

## MLT Get Interesting Terms
```
http://localhost:8983/solr/movie_content/select?mlt=true&mlt.mintf=1&mlt.interestingTerms=details&mlt.boost=true&mlt.fl=original_title,description,genre&q=imdb_title_id:tt0120737&fl=original_title,score&mlt.count=10
```

## Final Query (Q)
    avg_vote:[7.3 TO 10.3]^40
    genre:Drama^30
    genre:Action^30
    genre:Adventure^30
    description:one
    description:set
    description:save
    description:journey
    description:middle
    description:dark
    description:earth
    description:powerful
    description:destroy
    description:lord
    description:ring
    description:eight
    description:companions
    description:meek
    description:hobbit
    description:shire
    description:sauron 
    original_title:fellowship^20
    original_title:ring^20
    original_title:lord^20
    original_title:rings^20
 

