---
layout: post
title: "records: SQL for Humans"
comments: true
description: "exploring Kenneth Reitz's records library"
keywords: "python"
---

> Kenneth Reitz is considered a python library design genius and altogether great API designer. Today I read through records.py.

## The code

It's all [one file](https://github.com/kennethreitz/records/blob/master/records.py). I'll admit, I didn't get to the tests yet... The library relies on tablib and SQL Alchemy to provide tabular data structures and SQL database connection. Essentially, this wraps up your db connection, and gives you an easy interface to the query cursors.

On first read, I did NOT get that `RecordCollection` is an `Iterator`, not just an `Iterable`. It's meant to very quickly have `first()` or `all()` on it unless you're doing something complicated.

Figuring out the relationship between `_all_rows` and `_rows` and when they are populated was tricky. An important thing to know is that `list(object)` calls the iterator on `object`.

This is an example of python taking two libraries with different APIs and designs, and a really really smart API designer combining them to make them MORE elegant.


