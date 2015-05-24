title: "java generics tips and tricks"
date: 2015-05-12 05:54:26
tags: java
---

Parameterized type: `List<String>`

Actual type parameter: `String`

Generic type: `List<E>`

Formal type parameter: `E`

Unbounded wildcard type: `List<?>`

Raw type: `List`

Bounded type parameter: `<E extends Number>`

Recursive type bound: `<T extends Comparable<T>>`

Bounded wildcard type: `Bounded wildcard type`

List<? extends Number>: `static <E> List<E> asList(E[] a)`

Type token: `String.class`

<!--more-->


References:
1: http://docs.oracle.com/javase/tutorial/java/generics/index.html
2: http://www.angelikalanger.com/GenericsFAQ/JavaGenericsFAQ.html
3: http://www.jot.fm/issues/issue_2004_12/article5/
