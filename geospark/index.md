---
layout: post
title: Using GeoSpark for KNN
---

![GeoSpark logo]({{ site.url }}/images/geospark/geospark_logo.png)


Hi! 

As a part of a Kaggle competition [Sberbank Russian Housing Market](https://www.kaggle.com/c/sberbank-russian-housing-market)
I was looking for a way of how to find out missing values for ecology feature for some real estate properties. 
Thanks to Chuppy kaggler we've got pretty accurate approximate GPS coordinates for all out training and data locations.

Since I'm using Spark MLLib heavily I needed some tool that works with geographical data and knows what RDD is at the same time.
Luckily, I've tripped over GeoSpark library.
 
 I would like to share experience and some knowledge that I managed to collect while solving my task.
 
 First of all, here is a link to [https://github.com/DataSystemsLab/GeoSpark](GeoSpark) library.
  
  For sbt add this to you project's dependencies:
 
 ``` "org.datasyslab" % "geospark" % "0.7.1-snapshot", ```
 
 GeoSpark is written on Java and you will not find
  [https://github.com/DataSystemsLab/GeoSpark/blob/master/core/src/main/java/org/datasyslab/geospark/showcase/Example.java](examples) for Scala.
   
   To be honest there is a lack of good examples( tests) out there and this fact partially forces me to write this post.
   
   Let's back to the task, I needed to implement K-nearest neighbours using geographical distance as a distance for this algorithm.
   
   Here is a test from Example.java that do close to what I needed:
   
   ```
    public static void testSpatialKnnQuery() throws Exception {
       	objectRDD = new PointRDD(sc, PointRDDInputLocation, PointRDDOffset, PointRDDSplitter, true, StorageLevel.MEMORY_ONLY());
       	objectRDD.rawSpatialRDD.persist(StorageLevel.MEMORY_ONLY());
       	for(int i=0;i<eachQueryLoopTimes;i++)
       	{
       		List<Point> result = KNNQuery.SpatialKnnQuery(objectRDD, kNNQueryPoint, 1000,false);
       		assert result.size()>-1;
       	}
    }
    ```   
    
    Analog on Scala:
    
    ```
        val pointRddOffset = 1
        val pointRDDSplitter: FileDataSplitter = FileDataSplitter.CSV
        val inputLocationCSV = System.getProperty("user.dir")+"/src/main/scala/com/engineering/ecology/" + "id_lat_lon_nonmissing.csv"
    
        val objectRDD = new PointRDD(ss.sparkContext, inputLocationCSV, pointRddOffset, pointRDDSplitter, true, StorageLevel.MEMORY_ONLY)
        objectRDD.rawSpatialRDD.persist(StorageLevel.MEMORY_ONLY)
        val geometryFactory = new GeometryFactory()
        
    ```    
   
   Now, one row at a time. 
   
   ```val pointRddOffset = 1``` 
   
   This value should be equal to an index(shift) of the coordinates in the input csv file. 
   
   In my case I set it to `1` because of the `id` attribute that comes before latitude and longitude.
   
   Here is first three row of my input file:
   
   ```
   1,55.8910074613746,37.6048439354222
   2,55.6769988186591,37.6731346317014
   3,55.7029463354658,37.7411592517173
   ...
   ```
   
   Notice that file shouldn't have header row and that latitude comes first.
   
   ``` val pointRDDSplitter: FileDataSplitter = FileDataSplitter.CSV ```
   
   Here we just tell to GeoSpark that it should use splitter for .csv files.
   
   Next, we should specify path to the file with our coordinates. Unfortunately I didn't find a way to pass RDD 
   to the constructor of PointRDD.
   
   ```
   val objectRDD = new PointRDD(ss.sparkContext, inputLocationCSV, pointRddOffset, pointRDDSplitter, true, StorageLevel.MEMORY_ONLY)
           
   ```
   This row will load **inputLocationCSV** file to PointRDD representation.
   
   
   
   
   
   
   
   
 
 
 
 
 


```
val pointRddOffset = 1
    val pointRDDSplitter: FileDataSplitter = FileDataSplitter.CSV
    val inputLocationCSV = System.getProperty("user.dir")+"/src/main/scala/com/geo/material/lat_lon_with_material.csv"

    val objectRDD = new PointRDD(ss.sparkContext, inputLocationCSV, pointRddOffset, pointRDDSplitter, true, StorageLevel.MEMORY_ONLY)
    objectRDD.rawSpatialRDD.persist(StorageLevel.MEMORY_ONLY)
    val geometryFactory = new GeometryFactory()
```

Really appreciate what XXX is doing for this project. He answers to my question really quickly. 