date: "2016-02-14"
categories: 
  - 大数据
title: Hadoop在Hadoop环境中运行
tags : 
 - hadoop
---


下面是一个完整的在Hadoop环境下运行WordCount的程序，相关命令以及在使用过程中遇到的问题都有详细的说明

前提：已经在本机正确部署了Hadoop环境（单机或者伪分布式都可以）

相比起直接在eclipse中运行，这个更有意义，也更有成就感

本文参考了Hadoop官方文档[MapReduceTutorial](file:///Users/liuyiyou/Desktop/hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html#Purpose)



```sh


➜  hadoop-2.6.0  bin/hdfs fs -mkdir liuyiyou
错误: 找不到或无法加载主类 fs 的  （这里是因为没有设置环境变量或者没有将tools.jar复制到hadoop中，具体可以参考环境变量设置和tools.jar的位置。）
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir liuyiyou （设置HDFS目录，可以通过相关命令进行查看）
16/02/12 17:50:43 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
mkdir: `liuyiyou': No such file or directory
➜  hadoop-2.6.0  bin/hdfs dfs  -copyFromLocal input/test.txt /user/liuyiyou/test.txt（将本地文件复制到HDFS目录中）
16/02/12 17:51:57 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
copyFromLocal: `/user/liuyiyou/test.txt': No such file or directory
➜  hadoop-2.6.0  bin/hdfs dfs  -copyFromLocal input/test.txt /liuyiyou/test.txt
16/02/12 17:52:39 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
copyFromLocal: `/liuyiyou/test.txt': No such file or directory
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir /user
16/02/12 17:53:35 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir /user/liuyiyou
16/02/12 17:54:18 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs  -copyFromLocal input/test.txt /user/liuyiyou/test.txt
16/02/12 17:54:47 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs -ls                  （查看HDFS目录中的文件）
16/02/12 17:55:40 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Found 1 items
-rw-r--r--   1 liuyiyou supergroup        774 2016-02-12 17:54 test.txt
➜  hadoop-2.6.0  bin/hdfs dfs -ls .（查看HDFS目录中的文件）
16/02/12 17:56:05 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Found 1 items
-rw-r--r--   1 liuyiyou supergroup        774 2016-02-12 17:54 test.txt
➜  hadoop-2.6.0  bin/hdfs -mkdir books
Unrecognized option: -mkdir
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir books
16/02/12 17:58:00 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs -ls .
16/02/12 17:58:26 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Found 2 items
drwxr-xr-x   - liuyiyou supergroup          0 2016-02-12 17:58 books
-rw-r--r--   1 liuyiyou supergroup        774 2016-02-12 17:54 test.txt

（这里开始运行文档中的WordCount程序）

➜  hadoop-2.6.0  bin/hadoop com.sum.tools.javac.Main WordCount.java
错误: 找不到或无法加载主类 com.sum.tools.javac.Main（需要设置环境变量或者复制tools.jar到hadoop中）
➜  hadoop-2.6.0  cd ~
➜  ~  source .bash_profile
➜  ~  cd hadoop-2.6.0
➜  hadoop-2.6.0  bin/hadoop com.sum.tools.javac.Main WordCount.java
错误: 找不到或无法加载主类 com.sum.tools.javac.Main
➜  hadoop-2.6.0  bin/hadoop com.sum.tools.javac.Main WordCount.java
错误: 找不到或无法加载主类 com.sum.tools.javac.Main
➜  hadoop-2.6.0  bin/hadoop com.sum.tools.javac.Main WordCount.java
错误: 找不到或无法加载主类 com.sum.tools.javac.Main
➜  hadoop-2.6.0  bin/hadoop com.sum.tools.javac.Main WordCount.java
错误: 找不到或无法加载主类 com.sum.tools.javac.Main

➜  hadoop-2.6.0  bin/hadoop com.sun.tools.javac.Main WordCount.java（这里已经设置好之后不报错，编译程序）
➜  hadoop-2.6.0  jar cf wc.jar WordCount*.class（打包程序）
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir /user/joe/wordcount/input（设置HDFS输入目录，只能逐个设置）
16/02/13 01:27:27 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
mkdir: `/user/joe/wordcount/input': No such file or directory
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir /user/joe/
16/02/13 01:27:55 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir /user/joe/wordcount
16/02/13 01:28:19 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir /user/joe/wordcount/input（设置HDFS目录完成）
16/02/13 01:28:40 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs -mkdir /user/joe/wordcount/output（这里不需要设置，输出如果没有，会自动生成，这样导致后面又将该目录删除了）
16/02/13 01:29:03 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs  -copyFromLocal input/file01 /user/joe/wordcount/input（将本地文件复制到HDFS目录）
16/02/13 01:33:04 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hdfs dfs  -copyFromLocal input/file02 /user/joe/wordcount/input
16/02/13 01:33:38 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
➜  hadoop-2.6.0  bin/hadoop jar wc.jar WordCount /user/joe/wordcount/input /user/joe/wordcount/output（运行程序失败，原因是output目录已经存在）

16/02/13 01:34:10 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
16/02/13 01:34:10 INFO Configuration.deprecation: session.id is deprecated. Instead, use dfs.metrics.session-id
16/02/13 01:34:10 INFO jvm.JvmMetrics: Initializing JVM Metrics with processName=JobTracker, sessionId=
Exception in thread "main" org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory hdfs://localhost:9000/user/joe/wordcount/output already exists
at org.apache.hadoop.mapreduce.lib.output.FileOutputFormat.checkOutputSpecs(FileOutputFormat.java:146)
at org.apache.hadoop.mapreduce.JobSubmitter.checkSpecs(JobSubmitter.java:562)
at org.apache.hadoop.mapreduce.JobSubmitter.submitJobInternal(JobSubmitter.java:432)
at org.apache.hadoop.mapreduce.Job$10.run(Job.java:1296)
at org.apache.hadoop.mapreduce.Job$10.run(Job.java:1293)
at java.security.AccessController.doPrivileged(Native Method)
at javax.security.auth.Subject.doAs(Subject.java:415)
at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
at org.apache.hadoop.mapreduce.Job.submit(Job.java:1293)
at org.apache.hadoop.mapreduce.Job.waitForCompletion(Job.java:1314)
at WordCount.main(WordCount.java:59)
at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
at java.lang.reflect.Method.invoke(Method.java:606)
at org.apache.hadoop.util.RunJar.run(RunJar.java:221)
at org.apache.hadoop.util.RunJar.main(RunJar.java:136)
➜  hadoop-2.6.0  bin/hdfs dfs -remove /user/joe/wordcount/output
-remove: Unknown command
➜  hadoop-2.6.0  bin/hdfs dfs -rm /user/joe/wordcount/output（删除output目录，但是失败）

16/02/13 01:38:18 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
rm: `/user/joe/wordcount/output': Is a directory
➜  hadoop-2.6.0  bin/hdfs dfs -rm -R  /user/joe/wordcount/output（删除成功）

16/02/13 01:39:15 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
16/02/13 01:39:16 INFO fs.TrashPolicyDefault: Namenode trash configuration: Deletion interval = 0 minutes, Emptier interval = 0 minutes.
Deleted /user/joe/wordcount/output
➜  hadoop-2.6.0  bin/hadoop jar wc.jar WordCount /user/joe/wordcount/input /user/joe/wordcount/output（运行成功）

16/02/13 01:39:46 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
16/02/13 01:39:47 INFO Configuration.deprecation: session.id is deprecated. Instead, use dfs.metrics.session-id
16/02/13 01:39:47 INFO jvm.JvmMetrics: Initializing JVM Metrics with processName=JobTracker, sessionId=
16/02/13 01:39:47 WARN mapreduce.JobSubmitter: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
16/02/13 01:39:47 INFO input.FileInputFormat: Total input paths to process : 2
16/02/13 01:39:47 INFO mapreduce.JobSubmitter: number of splits:2
16/02/13 01:39:48 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_local108329066_0001
16/02/13 01:39:48 INFO mapreduce.Job: The url to track the job: http://localhost:8080/
16/02/13 01:39:48 INFO mapreduce.Job: Running job: job_local108329066_0001
16/02/13 01:39:48 INFO mapred.LocalJobRunner: OutputCommitter set in config null
16/02/13 01:39:48 INFO mapred.LocalJobRunner: OutputCommitter is org.apache.hadoop.mapreduce.lib.output.FileOutputCommitter
16/02/13 01:39:48 INFO mapred.LocalJobRunner: Waiting for map tasks
16/02/13 01:39:48 INFO mapred.LocalJobRunner: Starting task: attempt_local108329066_0001_m_000000_0
16/02/13 01:39:48 INFO util.ProcfsBasedProcessTree: ProcfsBasedProcessTree currently is supported only on Linux.
16/02/13 01:39:48 INFO mapred.Task:  Using ResourceCalculatorProcessTree : null
16/02/13 01:39:48 INFO mapred.MapTask: Processing split: hdfs://localhost:9000/user/joe/wordcount/input/file02:0+27
16/02/13 01:39:48 INFO mapred.MapTask: (EQUATOR) 0 kvi 26214396(104857584)
16/02/13 01:39:48 INFO mapred.MapTask: mapreduce.task.io.sort.mb: 100
16/02/13 01:39:48 INFO mapred.MapTask: soft limit at 83886080
16/02/13 01:39:48 INFO mapred.MapTask: bufstart = 0; bufvoid = 104857600
16/02/13 01:39:48 INFO mapred.MapTask: kvstart = 26214396; length = 6553600
16/02/13 01:39:48 INFO mapred.MapTask: Map output collector class = org.apache.hadoop.mapred.MapTask$MapOutputBuffer
16/02/13 01:39:48 INFO mapred.LocalJobRunner:
16/02/13 01:39:48 INFO mapred.MapTask: Starting flush of map output
16/02/13 01:39:48 INFO mapred.MapTask: Spilling map output
16/02/13 01:39:48 INFO mapred.MapTask: bufstart = 0; bufend = 44; bufvoid = 104857600
16/02/13 01:39:48 INFO mapred.MapTask: kvstart = 26214396(104857584); kvend = 26214384(104857536); length = 13/6553600
16/02/13 01:39:48 INFO mapred.MapTask: Finished spill 0
16/02/13 01:39:48 INFO mapred.Task: Task:attempt_local108329066_0001_m_000000_0 is done. And is in the process of committing
16/02/13 01:39:49 INFO mapred.LocalJobRunner: map
16/02/13 01:39:49 INFO mapred.Task: Task 'attempt_local108329066_0001_m_000000_0' done.
16/02/13 01:39:49 INFO mapred.LocalJobRunner: Finishing task: attempt_local108329066_0001_m_000000_0
16/02/13 01:39:49 INFO mapred.LocalJobRunner: Starting task: attempt_local108329066_0001_m_000001_0
16/02/13 01:39:49 INFO util.ProcfsBasedProcessTree: ProcfsBasedProcessTree currently is supported only on Linux.
16/02/13 01:39:49 INFO mapred.Task:  Using ResourceCalculatorProcessTree : null
16/02/13 01:39:49 INFO mapred.MapTask: Processing split: hdfs://localhost:9000/user/joe/wordcount/input/file01:0+21
16/02/13 01:39:49 INFO mapred.MapTask: (EQUATOR) 0 kvi 26214396(104857584)
16/02/13 01:39:49 INFO mapred.MapTask: mapreduce.task.io.sort.mb: 100
16/02/13 01:39:49 INFO mapred.MapTask: soft limit at 83886080
16/02/13 01:39:49 INFO mapred.MapTask: bufstart = 0; bufvoid = 104857600
16/02/13 01:39:49 INFO mapred.MapTask: kvstart = 26214396; length = 6553600
16/02/13 01:39:49 INFO mapred.MapTask: Map output collector class = org.apache.hadoop.mapred.MapTask$MapOutputBuffer
16/02/13 01:39:49 INFO mapred.LocalJobRunner:
16/02/13 01:39:49 INFO mapred.MapTask: Starting flush of map output
16/02/13 01:39:49 INFO mapred.MapTask: Spilling map output
16/02/13 01:39:49 INFO mapred.MapTask: bufstart = 0; bufend = 38; bufvoid = 104857600
16/02/13 01:39:49 INFO mapred.MapTask: kvstart = 26214396(104857584); kvend = 26214384(104857536); length = 13/6553600
16/02/13 01:39:49 INFO mapred.MapTask: Finished spill 0
16/02/13 01:39:49 INFO mapred.Task: Task:attempt_local108329066_0001_m_000001_0 is done. And is in the process of committing
16/02/13 01:39:49 INFO mapred.LocalJobRunner: map
16/02/13 01:39:49 INFO mapred.Task: Task 'attempt_local108329066_0001_m_000001_0' done.
16/02/13 01:39:49 INFO mapred.LocalJobRunner: Finishing task: attempt_local108329066_0001_m_000001_0
16/02/13 01:39:49 INFO mapred.LocalJobRunner: map task executor complete.
16/02/13 01:39:49 INFO mapred.LocalJobRunner: Waiting for reduce tasks
16/02/13 01:39:49 INFO mapred.LocalJobRunner: Starting task: attempt_local108329066_0001_r_000000_0
16/02/13 01:39:49 INFO util.ProcfsBasedProcessTree: ProcfsBasedProcessTree currently is supported only on Linux.
16/02/13 01:39:49 INFO mapred.Task:  Using ResourceCalculatorProcessTree : null
16/02/13 01:39:49 INFO mapred.ReduceTask: Using ShuffleConsumerPlugin: org.apache.hadoop.mapreduce.task.reduce.Shuffle@a24b738
16/02/13 01:39:49 INFO reduce.MergeManagerImpl: MergerManager: memoryLimit=333971456, maxSingleShuffleLimit=83492864, mergeThreshold=220421168, ioSortFactor=10, memToMemMergeOutputsThreshold=10
16/02/13 01:39:49 INFO reduce.EventFetcher: attempt_local108329066_0001_r_000000_0 Thread started: EventFetcher for fetching Map Completion Events
16/02/13 01:39:49 INFO reduce.LocalFetcher: localfetcher#1 about to shuffle output of map attempt_local108329066_0001_m_000000_0 decomp: 41 len: 45 to MEMORY
16/02/13 01:39:49 INFO reduce.InMemoryMapOutput: Read 41 bytes from map-output for attempt_local108329066_0001_m_000000_0
16/02/13 01:39:49 INFO reduce.MergeManagerImpl: closeInMemoryFile -> map-output of size: 41, inMemoryMapOutputs.size() -> 1, commitMemory -> 0, usedMemory ->41
16/02/13 01:39:49 INFO reduce.LocalFetcher: localfetcher#1 about to shuffle output of map attempt_local108329066_0001_m_000001_0 decomp: 36 len: 40 to MEMORY
16/02/13 01:39:49 INFO reduce.InMemoryMapOutput: Read 36 bytes from map-output for attempt_local108329066_0001_m_000001_0
16/02/13 01:39:49 INFO reduce.MergeManagerImpl: closeInMemoryFile -> map-output of size: 36, inMemoryMapOutputs.size() -> 2, commitMemory -> 41, usedMemory ->77
16/02/13 01:39:49 INFO reduce.EventFetcher: EventFetcher is interrupted.. Returning
16/02/13 01:39:49 INFO mapred.LocalJobRunner: 2 / 2 copied.
16/02/13 01:39:49 INFO reduce.MergeManagerImpl: finalMerge called with 2 in-memory map-outputs and 0 on-disk map-outputs
16/02/13 01:39:49 INFO mapred.Merger: Merging 2 sorted segments
16/02/13 01:39:49 INFO mapred.Merger: Down to the last merge-pass, with 2 segments left of total size: 61 bytes
16/02/13 01:39:49 INFO reduce.MergeManagerImpl: Merged 2 segments, 77 bytes to disk to satisfy reduce memory limit
16/02/13 01:39:49 INFO reduce.MergeManagerImpl: Merging 1 files, 79 bytes from disk
16/02/13 01:39:49 INFO reduce.MergeManagerImpl: Merging 0 segments, 0 bytes from memory into reduce
16/02/13 01:39:49 INFO mapred.Merger: Merging 1 sorted segments
16/02/13 01:39:49 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 69 bytes
16/02/13 01:39:49 INFO mapred.LocalJobRunner: 2 / 2 copied.
16/02/13 01:39:49 INFO Configuration.deprecation: mapred.skip.on is deprecated. Instead, use mapreduce.job.skiprecords
16/02/13 01:39:49 INFO mapred.Task: Task:attempt_local108329066_0001_r_000000_0 is done. And is in the process of committing
16/02/13 01:39:49 INFO mapred.LocalJobRunner: 2 / 2 copied.
16/02/13 01:39:49 INFO mapred.Task: Task attempt_local108329066_0001_r_000000_0 is allowed to commit now
16/02/13 01:39:49 INFO output.FileOutputCommitter: Saved output of task 'attempt_local108329066_0001_r_000000_0' to hdfs://localhost:9000/user/joe/wordcount/output/_temporary/0/task_local108329066_0001_r_000000
16/02/13 01:39:49 INFO mapred.LocalJobRunner: reduce > reduce
16/02/13 01:39:49 INFO mapred.Task: Task 'attempt_local108329066_0001_r_000000_0' done.
16/02/13 01:39:49 INFO mapred.LocalJobRunner: Finishing task: attempt_local108329066_0001_r_000000_0
16/02/13 01:39:49 INFO mapred.LocalJobRunner: reduce task executor complete.
16/02/13 01:39:49 INFO mapreduce.Job: Job job_local108329066_0001 running in uber mode : false
16/02/13 01:39:49 INFO mapreduce.Job:  map 100% reduce 100%
16/02/13 01:39:49 INFO mapreduce.Job: Job job_local108329066_0001 completed successfully
16/02/13 01:39:49 INFO mapreduce.Job: Counters: 35
File System Counters
FILE: Number of bytes read=10860

FILE: Number of bytes written=772228

FILE: Number of read operations=0

FILE: Number of large read operations=0

FILE: Number of write operations=0

HDFS: Number of bytes read=123

HDFS: Number of bytes written=41

HDFS: Number of read operations=25

HDFS: Number of large read operations=0

HDFS: Number of write operations=5

Map-Reduce Framework
Map input records=2

Map output records=8

Map output bytes=82

Map output materialized bytes=85

Input split bytes=236

Combine input records=8

Combine output records=6

Reduce input groups=5

Reduce shuffle bytes=85

Reduce input records=6

Reduce output records=5

Spilled Records=12

Shuffled Maps =2

Failed Shuffles=0

Merged Map outputs=2

GC time elapsed (ms)=13

Total committed heap usage (bytes)=951058432

Shuffle Errors
BAD_ID=0

CONNECTION=0

IO_ERROR=0

WRONG_LENGTH=0

WRONG_MAP=0

WRONG_REDUCE=0

File Input Format Counters
Bytes Read=48

File Output Format Counters
Bytes Written=41

➜  hadoop-2.6.0  bin/hdfs dfs -cat /user/joe/wordcount/output/part-r-00000
16/02/13 01:40:48 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable（查看结果）

Bye 1
Goodbye 1
Hadoop 2
Hello 2
World 2

```

java文件：，位置：/hadoop-2.6.0/

```java

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class WordCount {

  public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }

  public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}

```