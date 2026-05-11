---
title: nosql-databases
tags: [nosql]
draft: false
date: 2026-04-25
---
4 types of databases in nosql:
- document store
	- document is a unit that is flexible 
	
- wide column database
- key-value store
- graph database


|          | document store                                                                                                                                                       | wide column database                                                        | key-value store                                                                                                                                                                        | graph database                                                                                                                                                                          |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|          | store data in semi-structured records, using formats like json, bson or xml --> highly flexible, allow store data in a format similar to application's code objects  | extreme scalability, efficient data compression, ideal for massive datasets | every data item is stored as a unique key paired with a corresponding vlaue<br>--> optimized for lightning-fast read and write operations but are limited in terms of complex querying | focuses on relationships btw data points by using "nodes" (entities) and "edges" (relationships) --> for traverse complex, highly connected data much faster than traditional sql joins |
| usage    | content management systems (CMS), ecommerce catalogs, user profiles                                                                                                  | large-scale data warehousing, real-time analysis, iot sensor data           | caching, session management, real-time leaderboards                                                                                                                                    | social networks, fraud detection, recommendation engines                                                                                                                                |
| exmaples | mongodb, apache couchdb, firebase firestore                                                                                                                          |                                                                             | redis, amazon dynamodb, memcached                                                                                                                                                      | neo4j, amazon neptune, arangodb                                                                                                                                                         |


mongodb:
- basc unit data: ddocument
- collection : containers for documents 
- schema flexible -
- mongosh


when is a nosql >> relational db? 
- when handling unstructured, dynamic data
- massive scalability needs
- rapid devlopment cycles 
- simpe data retrieval 
- large-scale performance 