---
layout: default
title: On JNI
parent: My Thoughts
---

# JNI
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Java's Native Interface or **JNI** for short is a mechanism that provides for a bridge between the Java bytecode running in a JVM and native code, in this case written in C or C++, on a given platform. There are plenty of examples of how to pass `int`s, `float`s, arrays of them, or even Java `String`s. However, there seem to be a dearth of examples on how to deal with Java objects, collections, `EnumSet`s, etc.  Hopefully the examples that I provide will allow you to get back to making progress towards your ultimate goal of completing your project.
{: .fs-5 .fw-300 }
