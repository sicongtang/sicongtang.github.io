---
layout: post
title: mysql high performance - datatypes/indexing
date: 2014-11-25 00:19:32
categories: books
---

##Choosing Optimal Data Types
* Smaller is usually better.
* Simple is good. For example, integers are cheaper to compare than characters, because character sets and collations (sorting rules) make character comparisons compli- cated.
* Avoid NULL if possible.

####
####VARCHAR and CHAR types
####BLOB and TEXT types
####Using ENUM instead of a string type
####DATETIME and TIMESTAMP types
####Bit-Packed Data Types

##A Mixture of Normalized and Denormalized
The most common way to denormalize data is to duplicate, or cache, selected columns from one table in another table. In MySQL 5.0 and newer, you can use triggers to update the cached values, which makes the implementation easier.
##Cache and Summary Tables
####Materialized Views
####Counter Tables

##Types of Indexes

