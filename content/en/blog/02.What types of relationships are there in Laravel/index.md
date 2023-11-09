---
title: "What types of relationships are there in Laravel?"
date: 2023-08-10
draft: false
slug: "what-types-of-relationships-are-there-in-laravel"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 1
---

Often, developers need to interact with databases. If you are using the Laravel framework, you should know about one of its most important features: Eloquent, an object-relational mapper (ORM) that makes this process simple and easy.

__Laravel Eloquent__ is one of the main features in the __Laravel framework__. This is due to its great support for defining, creating, and managing relationships between different tables. In this series of articles, I will show you how to create and use __Eloquent relationships__.

It is important to note that you can start using Eloquent without any prior knowledge of relationships.

As a professional programmer, it is necessary to understand the types of relationships. However, before that, you must ask yourself an important question: what are relationships in the first place?

## What are relationships in databases?
![What are relationships in databases?](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/what-are-relationships-in-database.png "What are relationships in databases?")

{{< youtube id="MFhbyjZlwH0" title="What are relationships in databases?" >}}
When working with tables in a database that have relationships between them, we can describe these relationships as links between those tables. This helps you organize and structure data effortlessly, allowing for faster data reading and processing.

## What types of relationships are there in Laravel?
![What types of relationships are there in Laravel?](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/what-types-of-relationships-are-there-in-laravel.png "What types of relationships are there in Laravel?")

There are three main types of relationships in databases that are commonly used in practice:

* __one-to-one__:  A one-to-one relationship means that one record in one table can only be related to one record in another table. For example, a customer table might have a one-to-one relationship with an address table. This means that each customer can only have one address, and each address can only be associated with one customer.

* __one-to-many__: A one-to-many relationship means that one record in one table can be related to many records in another table. For example, an order table might have a one-to-many relationship with a product table. This means that one order can have many products, but each product can only be associated with one order.

* __many-to-many__:  A many-to-many relationship means that many records in one table can be related to many records in another table. For example, a student table might have a many-to-many relationship with a course table. This means that many students can take many courses, and many courses can be taken by many students.

Besides these relationships, __Laravel__ offers more relationships, namely:

* __Has Many Through__: This relationship allows you to relate a model to another model through a third model. For example, a user model might have a has many through relationship with a post model, where the third model is a category model. This means that a user can have many posts, where each post belongs to a category.

* __Polymorphic Relations__: This relationship allows you to relate a model to multiple other models of different types. For example, a comment model might have a polymorphic relationship with a model, where the model could be a blog post, a product, or a user. This means that a comment can be associated with any of these three types of models.

* __Many-to-many Polymorphic__: This relationship is a combination of the has many through and polymorphic relationships. It allows you to relate a model to many other models of different types, where the relationship is mediated by a third model. For example, a user model might have a many-to-many polymorphic relationship with a model, where the third model is a role model. This means that a user can have many roles, where each role could be a user, a product, or a blog post.

The number of relationships that we will explain is 6 types. We will build a simple content management system to explain all of those relationships.

> Do I need specific knowledge of Eloquent before reading this?

In response to your question about whether you need specific knowledge of Eloquent before reading this, I would say that it is not strictly necessary, but it would be helpful. The examples in the article do not use any complex Eloquent methods or techniques, but it would be helpful to have a basic understanding of how Eloquent works before reading the article.

If you are new to Laravel, I would recommend reading the Laravel documentation: https://laravel.com/docs/ before reading this article. The documentation provides a good overview of Eloquent and how it works.

Once you have a basic understanding of Eloquent, you should be able to follow the examples in the article without any problems. However, if you get stuck, you can always refer to the documentation for more help.


In the examples below, I have tried to explain everything as clearly as possible, without using too many tricky Eloquent methods and complex techniques. This means that prior knowledge is not strictly necessary. However, it is always best to learn the basics first and then pursue more complex topics such as relationships.