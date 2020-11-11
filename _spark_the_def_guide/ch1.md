---
layout: post
title: Ch.1 - What is Apache Spark?
tags: ['sparkdefguide']
order: 1
comments: true
---

# Spark's toolkit
<table style="border-collapse: collapse; min-width: 100%;"><colgroup><col style="width: 130px;"><col style="width: 130px;"><col style="width: 130px;"></colgroup>
	<tbody>
		<tr>
		<td style="width: 130px; padding: 8px; border: 1px solid;">
		<div style="text-align: center; ">Structured Streaming</div>
		</td>
		<td style="width: 130px; padding: 8px; border: 1px solid;">
		<div style="text-align: center; ">Advanced Analytics</div>
		</td>
		<td style="width: 130px; padding: 8px; border: 1px solid;">
		<div style="text-align: center; ">Libraries & Ecosystem</div>
		</td>
		</tr>
		<tr>
		<td colspan="3" style="width: 390px; padding: 8px; border: 1px solid;">
		<div style="text-align: center; ">Structured API</div>
		<div style="text-align: center; ">Datasets, DataFrames, SQL</div>
		</td>
		</tr>
		<tr>
		<td colspan="3" style="width: 390px; padding: 8px; border: 1px solid;">
		<div style="text-align: center; ">Low-level APIs</div>
		<div style="text-align: center; ">RDD, Distributed Variables</div>
		</td>
		</tr>
	</tbody>
</table>
<!--more-->

# Unified aspect of Spark:
- Consistent, compassable APIs
- Unified engine for parallel data processing
- “Structured APIs” (Datasets, DataFrames, SQL)

# Computing engines:
- Azure Storage and Amazon S3
- Distributed file systems (e.g. Apache Hadoop)
- Key-value stores (e.g. Apache Cassandra)
- Message buses (e.g. Apache Kafka)
Spark focuses on performing computations over the data, no matter where it resides.

# Hadoop:
- Storage system: the Hadoop file system/HDFS designed for low-cost stage over clusters of commodity servers
- Computing system: MapReduce
Environments for which Hadoop architecture cannot work: public cloud/streaming application -> Spark can work on that too

# Libraries:
- SQL
- Structured data (Spark SQL)
- Machine learning (MLlib)
- Stream processing (Spark Streaming and newer Structured Streaming)
- Graph analysis (GraphX)

# Context: the big data problem
- Hardware advancement: (before 2005) computer became faster every year through processor speed increase
- The trend in hardware stopped around 2005 due to hard limit in heat dissipation
- Developer switch towards adding more parallel CPU cores all running at the same speed
- The cost to store 1TB of data continues to drop by roughly two times every 14 months
- Collecting data is extremely inexpensive
- Processing huge amount of data requires large, parallel computation, often on clusters of machines

# Running Spark
- Spark can be used from Python, Java, Scala, R, or SQL
- Spark is written in Scala, and runs on the Java Virtual Machine (JVM)
- To run Spark, one has to install Java