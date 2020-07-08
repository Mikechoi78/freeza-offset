# freeza-offset

<span style="display:block;text-align:center">![freeza-logo](assets/freeza-logo.png)</span>


## What is it?

**freeza-offset** is a Python package that provides a simple way to commit the offset consumed by Spark Streaming in Kafka's ConsumerGroup.

## Main Features
Here are just a few of the things that freeza-offset does well:

  - Commits the offset consumed in kafka
  - Tracking Spark consumption lag at Kafka
  - The offset is not just in control of the spark

## Where to get it
The source code is currently hosted on GitHub at:
https://github.com/HashLoad/freeza-offset

Binary installers for the latest released version are available at the [Python package index](https://pypi.org/project/freeza-offset) and on conda.

```sh
# conda
conda install freeza-offset
```

```sh
# or PyPI
pip install freeza-offset
```

## Dependencies
- [kafka-python](https://pypi.org/project/kafka-python)


## Installation from sources

In the `freeza-offset` directory (same one where you found this file after
cloning the git repo), execute:

```sh
python setup.py install
```

## License
[MIT](LICENSE)

## Notebook Example:

```shell
pip install py4j  
pip install freeza-offset
```

```python
import os
os.environ['PYSPARK_SUBMIT_ARGS'] = '--packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.0.0 pyspark-shell'
from pyspark.sql import SparkSession

spark = SparkSession \
    .builder \
    .appName("FreezaCommitTest") \
    .getOrCreate()
```

```python
df = spark \
  .readStream \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "kafka1:9092,kafka2:9092,kafka3:9092") \
  .option("subscribe", "topic-name") \
  .option("startingOffsets", "earliest") \
  .option("kafka.group.id", "spark-freeza-runner") \
  .load()
 ```

```python
df.selectExpr("key", "value")
```

```python
qry = df.writeStream \
    .format("console") \
    .option("truncate","false") \
    .start()
```

```python
import freeza
tr = freeza.start_commiter_thread(
    query=qry,
    bootstrap_servers=bootstrap_servers,
    group_id="spark-freeza-commiter"
)
```

```python
tr.isAlive()
```

```python
qry.stop()
```

## Getting Help

For usage questions, the best place to go to is open [new issue](https://github.com/HashLoad/freeza-offset/issues/new)

## Contributing to freeza-offset

All contributions, bug reports, bug fixes, documentation improvements, enhancements, and ideas are welcome.
