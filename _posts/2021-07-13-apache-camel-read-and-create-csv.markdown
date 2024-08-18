---
layout: post
title: "Apache Camel CSV"
date: 2021-07-13 05:15:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Apache Camel", "Apache Common CSV", "Apache Camel routes"]
permalink: /post/apache-camel-read-and-create-csv
---

## **Apache Camel**

Camel is an Open Source integration framework that empowers you to quickly and easily integrate various systems consuming or producing data. Camel supports most of the Enterprise Integration Patterns from the excellent book by Gregor Hohpe and Bobby Woolf, and newer integration patterns from microservice architectures to help you solve your integration problem by applying best practices out of the box.

Apache Camel is standalone, and can be embedded as a library within Spring Boot, Quarkus, Application Servers, and in the clouds. Camel subprojects focus on making your work easy.Packed with several hundred components that are used to access databases, message queues, APIs or basically anything under the sun. Helping you integrate with everything. Camel supports around 50 data formats, allowing to translate messages in multiple formats, and with support from industry standard formats from finance, telco, health-care, and more.

## **CSV**

The CSV Data Format uses Apache Common CSV to handle CSV payloads (Comma Separated Values) such as those exported/imported by Excel. Commons CSV reads and writes files in variations of the Comma Separated Value (CSV) format.

### Read & Create CSV

[https://youtu.be/fJ3iLNfKvR4](https://youtu.be/fJ3iLNfKvR4)

### **Dependencies**

To use CSV in your Camel routes you need to add a dependency on **camel-csv**, which implements this data format.

```xml
<dependency>
  <groupId>org.apache.camel</groupId>
  <artifactId>camel-csv</artifactId>
  <version>x.x.x</version>
</dependency>
```

### References

[https://camel.apache.org/components/latest/dataformats/csv-dataformat.html](https://camel.apache.org/components/latest/dataformats/csv-dataformat.html)