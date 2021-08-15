---
layout: page
title: Technical Notes
permalink: /ds_notes/
comments: true
---

It's fun to write about what I have learnt and share solutions to problems related to 
data science/machine learning I have solved.


## Machine Learning Journal Club

Summary slides I made for discussion in the *Machine Learning Journal Club* with peers.

{% for post in site.tags.machine_learning_journal_club %}
 - {{ post.date | date: "%Y-%m-%d" }} - [{{ post.title }}]({{ post.url }})
{% endfor %}

## Reading Notes

{% for post in site.tags.notesfromreading %}
 - {{ post.date | date: "%Y-%m-%d" }} - [{{ post.title }}]({{ post.url }})
{% endfor %}

## Software Engineering

{% for post in site.tags.se %}
 - {{ post.date | date: "%Y-%m-%d" }} - [{{ post.title }}]({{ post.url }})
{% endfor %}

## Data Science

{% for post in site.tags.ds %}
 - {{ post.date | date: "%Y-%m-%d" }} - [{{ post.title }}]({{ post.url }})
{% endfor %}

## System / Package set-up

{% for post in site.tags.setup %}
 - {{ post.date | date: "%Y-%m-%d" }} - [{{ post.title }}]({{ post.url }})
{% endfor %}

## Notes taken from lectures / online courses:

{% for post in site.coursenotes %}
 - {{ post.date | date: "%Y-%m-%d" }} - [{{ post.title }}]({{ post.url }})
{% endfor %}

