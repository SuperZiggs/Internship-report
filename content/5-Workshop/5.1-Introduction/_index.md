---
title : "Introduction to Iceberg on AWS"
date : 2026-03-25 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---


[Apache Iceberg](https://iceberg.apache.org/)  is an open table format for very large analytic datasets and supports modern data lake operations such as record-level insert, update, delete, and time travel queries. Iceberg manages large collections of files as tables, and it supports modern analytical data lake operations such as record-level insert, update, delete, and time travel queries. The Iceberg specification allows seamless table evolution such as schema and partition evolution, and its design is optimized for usage on Amazon S3. Iceberg also helps guarantee data correctness under concurrent write scenarios.

---

Some of the common terms in Apache Iceberg tables are as below:

-   Schema – Names and types of fields in a table.
-   Partition spec – A definition of how partition values are derived from data fields.
-   Snapshot – The state of a table at some point in time, including the set of all data files.
-   Manifest list – A file that lists manifest files; one per snapshot.
-   Manifest – A file that lists data or delete files; a subset of a snapshot.
-   Data file – A file that contains rows of a table.
-   Delete file – A file that encodes rows of a table that are deleted by position or data values.

---

Apache Iceberg tracks individual data files in a table instead of directories. This allows writers to create data files in-place and only adds files to the table in an explicit commit. The table state is maintained in metadata files. All changes to the table state create a new metadata file and atomically replaces the older metadata. The table metadata file tracks the table schema, partitioning config, other properties, and the snapshots of the table contents. A snapshot represents the state of a table at some time and is used to access the complete set of data files in the table.

Each snapshot is a complete set of data files in the table at a point in time. Snapshots are listed in the metadata file, but the files in a snapshot are stored in separate manifest files. The atomic transitions from one table metadata file to the next provide snapshot isolation. Readers use the snapshot that was current when they loaded the table metadata and are not affected by changes until they refresh and pick up a new metadata location. Paths to data files in snapshots are stored in one or more manifest files that contain a row for each data file in the table, its partition data, and its metrics. A snapshot is the union of all files in its manifests. Manifest files can also be shared between snapshots to avoid rewriting metadata that is slow-changing.

---

**Apache Iceberg on Amazon Athena**

Iceberg tables created against the AWS Glue catalog based on specifications defined by the open source [Glue Catalog implementation](https://iceberg.apache.org/docs/latest/aws/#glue-catalog)  are supported from Athena. Athena supports Iceberg v2 tables that are Parquet format only. To create an Iceberg table from Athena, set the `table_type table` property to `ICEBERG` in the `TBL_PROPERTIES` clause, as in the following syntax summary.

```sql
CREATE TABLE
  [db_name.]table_name (col_name data_type [COMMENT col_comment] [, ...] )
  [PARTITIONED BY (col_name | transform, ... )]
  LOCATION 's3://DOC-EXAMPLE-BUCKET/your-folder/'
  TBLPROPERTIES ( 'table_type' ='ICEBERG' [, property_name=property_value] )
```

---

**Apache Iceberg on Amazon EMR**

Starting with Amazon EMR 6.5.0, you can use Apache Spark 3 on Amazon EMR clusters with the Iceberg table format. EMR 6.5.0 supports the Iceberg version 0.12.0. You can create a cluster using the AWS Management Console, the AWS CLI, or the Amazon EMR API. To use Iceberg on Amazon EMR, create a cluster with the following classification:

```java
[{ "Classification":"iceberg-defaults",
    "Properties":{"iceberg.enabled":"true"}
}]
```

Alternatively, you can create an EMR cluster including the Spark application and include the file `/usr/share/aws/iceberg/lib/iceberg-spark3-runtime.jar` as a JAR dependency in a Spark jobs.

This workshop may take upto 2 hours. Use us-east-1 region for this workshop as the dataset we are working is in us-east-1 region.

