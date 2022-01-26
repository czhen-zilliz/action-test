---
id: 2019-11-08-data-management.md
title: Managing Data in Massive-Scale Vector Search Engine
author: Yihua Mo
date: 2019-11-08
desc:

cover:
tag: Engineering
origin: null
---

# Managing Data in Massive-Scale Vector Search Engine

> Author: Yihua Mo
>
> Date: 2019-11-08

## How data management is done in Milvus

First of all, some basic concepts of Milvus:

- Table: Table is a data set of vectors, with each vector having a unique ID. Each vector and its ID represent a row of the table. All vectors in a table must have the same dimensions. Below is an example of a table with 10-dimensional vectors:vectorsvectorsvectorsvectors
