# Dataset vs Dataframe

To perform Big Data processing we often use Dataset and Dataframes a lot. It is important to understand how they work and what is the point of difference between them.

To create a Dataset we import `org.apache.spark.sql.Dataset` class.

```java
import org.apache.spark.sql.Dataset;
```

Now let's create a `spark session`.

```java
SparkSession spark = new SparkSession.Builder()
                .appName("Array to Dataset<String>")
                .master("local")
                .getOrCreate();
```

The Dataset can take user defined types as well. If we declare `Dataset` with `Row` then it is referred to as `Dataframe`. If we try to declare `Dataset` with user-defined data type then it is named as `Dataset`.

The below code demonstrates declaring `Dataset` and `Dataframe`.

```java
/*
----------------------------------------
 Dataset with user-defined type Student
----------------------------------------
*/

// array of student names
String[] studentNames = new String[] {
    "Shashank J",
    "Shashank K L",
    "Superman",
    "Batman",
    "Purushotham"
};

// create a collection List
List<Student> data = Arrays.asList(studentNames);

Dataset<Student> ds = spark.createDataset (data, Encoders.STRING());
```

Now we try to print what's stored in `ds` and also print its `schema`.

```java
ds.printSchema();
```
#### ouput
```java
root
 |-- value: string (nullable = true)
```

and the contents of the Dataset are:

```java
+--------+
|   value|
+--------+
| Bannana|
|     Car|
|   Glass|
|Computer|
|     Car|
+--------+
```