---
layout: page
title: PySpark/SQL Solutions
permalink: /pyspark_solutions/
comments: true
---

I build machine learning models to handle huge amount of data on hadoop cluster with pyspark daily. Here are some solutions to problems with pyspark I solved:

## Pyspark-related blog posts

{% for post in site.tags.pyspark %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}

## SQL related blog posts

{% for post in site.tags.sql %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}
