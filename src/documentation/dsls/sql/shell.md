

# Beam SQL Shell Instructions

> To really use the Beam SQL Shell, you must assemble it yourself.

The Beam SQL Shell is in sdks/java/extensions/io

The Beam SQL Shell

. Out of the box, it supports only CSV files and the DirectRunner. You can add other IOs and runners by including them on the classpath.

To make it easy to 

These instructions are writing as though you build from source. If you have downloaded the jars another way, then adjust the paths.


```
./gradlew -p sdks/java/extensions/sql/shell -P beam.sql.shell.bundled=:beam-sdks-java-io-google-cloud-platform,:beam-sdks-java-io-kafka installDist
```


Try this:


```
$ ./sdks/java/extensions/sql/shell/build/install/beam-sdks-java-extensions-sql-shell/bin/beam-sdks-java-extensions-sql-shell

BeamSQL> SELECT 3, 'hello', DATE '2018-05-28';
+------------+--------+--------+
|   EXPR$0   | EXPR$1 | EXPR$2 |
+------------+--------+--------+
| 3          | hello  | 2018-05-28 |
+------------+--------+--------+
1 row selected (0.02 seconds)
```



```
$ cat > /tmp/my-test-file.csv << EOF
a,b,c,1,2,3
aa,bb,cc,11,22,33
EOF

BeamSQL> CREATE TABLE just_testing (
   a VARCHAR,
   b VARCHAR,
   c VARCHAR,
   x TINYINT,
   y INT,
   z BIGINT
)
TYPE text LOCATION '/tmp/my-test-file.csv';

No rows affected (0.248 seconds)

BeamSQL> SELECT * from just_testing;

+----+----+----+-----+------------+---------------------+
| a  | b  | c  |  x  |     y      |          z          |
+----+----+----+-----+------------+---------------------+
| a  | b  | c  | 1   | 2          | 3                   |
| aa | bb | cc | 11  | 22         | 33                  |
+----+----+----+-----+------------+---------------------+
2 rows selected (2.92 seconds)
```


To use another runner, you can build it add it to the classpath


```
$ ./gradlew :beam-runners-flink_2.11:shadowJar
$ java -cp runners/flink/build/libs/beam-runners-flink_2.11-2.6.0-SNAPSHOT-shaded.jar:sdks/java/extensions/sql/jdbc/build/libs/beam-sdks-java-extensions-sql-jdbc-2.6.0-SNAPSHOT-shaded.jar org.apache.beam.sdk.extensions.sql.jdbc.BeamSqlLine
BeamSQL> SET runner='FlinkRunner';
BeamSQL> CREATE TABLE just_testing (
   a VARCHAR,
   b VARCHAR,
   c VARCHAR,
   x TINYINT,
   y INT,
   z BIGINT
)
TYPE text LOCATION '/tmp/my-test-file.csv';

No rows affected (0.248 seconds)

BeamSQL> SELECT * from just_testing;
```


And wordcount

Try streaming via the public NYC taxi cab stream.

```

```

