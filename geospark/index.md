---
layout: post
title: Using GeoSpark for KNN
---

![GeoSpark logo]({{ site.url }}/images/geospark/geospark_logo.png)


Hi! As a part of a Kaggle competition [Sberbank Russian Housing Market](https://www.kaggle.com/c/sberbank-russian-housing-market)
I was looking for a way of how to find out missing values for ecology feature for some real estate properties. 
Thanks to Chuppy kaggler we've got pretty accurate approximate GPS coordinates for all out training and data locations.

Since I'm using Spark MLLib heavily I needed some tool that works with geographical data and knows what RDD is at the same time.
Luckily, I've tripped over GeoSpark library. I would like to share experience and some knowledge that I managed to collect
 while solving my task.
 
 


```
val pointRddOffset = 1
    val pointRDDSplitter: FileDataSplitter = FileDataSplitter.CSV
    val inputLocationCSV = System.getProperty("user.dir")+"/src/main/scala/com/geo/material/lat_lon_with_material.csv"

    val objectRDD = new PointRDD(ss.sparkContext, inputLocationCSV, pointRddOffset, pointRDDSplitter, true, StorageLevel.MEMORY_ONLY)
    objectRDD.rawSpatialRDD.persist(StorageLevel.MEMORY_ONLY)
    val geometryFactory = new GeometryFactory()
```