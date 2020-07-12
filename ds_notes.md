---
layout: page
title: Data Science Notes
permalink: /ds_notes/
comments: true
---

It's fun to write about what I have learnt and share solutions to problems related to 
data science/machine learning I have solved.

## Data Science

{% for post in site.tags.ds %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}

## Software Engineering

{% for post in site.tags.se %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}

## System / Package set-up

{% for post in site.tags.setup %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}

## Reading Notes/Summary

{% for post in site.tags.notesfromreading %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}

## Notes taken from lectures / online courses:

{% for post in site.coursenotes %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}

