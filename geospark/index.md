---
layout: post
title: Using GeoSpark for KNN
---

![GeoSpark logo]({{ site.url }}/images/geospark/geospark_logo.png)


```
val pointRddOffset = 1
    val pointRDDSplitter: FileDataSplitter = FileDataSplitter.CSV
    val inputLocationCSV = System.getProperty("user.dir")+"/src/main/scala/com/geo/material/lat_lon_with_material.csv"

    val objectRDD = new PointRDD(ss.sparkContext, inputLocationCSV, pointRddOffset, pointRDDSplitter, true, StorageLevel.MEMORY_ONLY)
    objectRDD.rawSpatialRDD.persist(StorageLevel.MEMORY_ONLY)
    val geometryFactory = new GeometryFactory()
```