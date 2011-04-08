# Avro Utils

Util library to be able to use Avro files as input and output of Hadoop Map/Reduce Jobs
or use Avro files as input of Hadoop Streaming.

## Avro I/O in Map/Reduce Jobs

### Avro input

Use `com.tomslabs.grid.avro.AvroFileInputFormat` to use Avro files as the *input* of Map/Reduce jobs.  

'map()' will be called with a key of type Avro's `GenericRecord` and a `NullWritable` value.

### Avro Ouput

Use `com.tomslabs.grid.avro.AvroFileOuputFormat` to use Avro files as the *output* of Map/Reduce jobs.  

In `reduce()` method, the context must emit with a key of type Avro's `GenericRecord` and  a `NullWritable` value.
Please note that the _Avro Schema for the output data MUST be specified when setting up the Job_.
 
### Example 

`src/test/java/com/tomslabs/grid/avro/AvroWordCount.java` is an example showing how to use Avro files for both input and output of a Map/Reduce jobs.

This example can be run as unit test from `AvroWordCountTest.java`.


## Avro Input for Hadoop Streaming

To use Avro files as input for Hadoop Streaming, use the jar generated by the project (available
through Debian package) and specify the correct input format:

    $ $HADOOP_HOME/bin/hadoop  jar $HADOOP_HOME/hadoop-streaming.jar \
        -libjars ./avro-utils-<VERSION>.jar,avro-1.4.1.jar  \
        -inputformat com.tomslabs.grid.avro.AvroTextFileInputFormat \
        -input <Avro file or dir> \
        -output <output dir> \
        -mapper <map command> \
        -reducer <reducer command>
     
For example, to count the number of expression, something like:

    $ $HADOOP_HOME/bin/hadoop  jar $HADOOP_HOME/hadoop-streaming.jar \
        -libjars ./avro-utils-1.5.1-SNAPSHOT.jar,avro-1.4.1.jar  \
        -inputformat com.tomslabs.grid.avro.AvroTextFileInputFormat \
        -input /tmp/word-count.avro \
        -output /tmp/out \
        -mapper /bin/cat \
        -reducer /bin/cat

The format of each line streamed through Avro looks like:

    <JSON representation of a Avro record>\t

(i.e. there is a trailing tabulation at the end of each line)

## Links

* [http://avro.apache.org/](http://avro.apache.org/)
* [http://wiki.apache.org/hadoop/HadoopStreaming](http://wiki.apache.org/hadoop/HadoopStreaming)
