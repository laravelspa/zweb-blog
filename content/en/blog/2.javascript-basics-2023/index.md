---
title: "HTML CSS And Javascript - 2023"
date: 2023-05-15
draft: true
slug: "html-css-javascript-2023"
cascade:
  showReadingTime: true
categories: ['Javascript']
tags: ['js']
---
## Introduction
- The Web
<!-- Image For Html Documents -->
<!-- 1.the_web image -->
hypertext: a form of text in which documents can refer (Link) To Other Documents And Resources
<!-- 2.the_link image -->
<!-- 3.the_hyperlink_refer_to_document Image -->
Which of these descriptions most accurately describes the Web?
HTML Documents and other resources that refer to each other with links.
  - Web Pages and Servers
  <!-- 4.the_web_pages_and_servers Image -->
  <!-- 5.http_protocol Image -->
  <!-- 6.build_a_web_page_for_first_time Image -->
Quiz:
1. When you go to a page on the Web, your browser sends a request to a server.
  - true
  - false

2.HTML stands for Hypertext Transfer Protocol.
  - true
  - false

3.HTTP stands for Hypertext Markup Language.
  - true
  - false

4.A web page can include resources (like videos or images) from multiple web servers.
  - true
  - false

5. Why do web addresses usually contain http: or https: at the beginning?
  - This tells the browser that the page is written in HTML.
  - This tells the browser that the page is written in English.
  - This tells the browser which rules (or protocol) to use when talking to the web server.

6. Suppose that you are working on a new web page on your own computer. What do you need to do to be able to see this web page displayed in your browser as you work on it?
  - Put the file on a web server and then load it in the browser.
  - Simply save the file on your own computer and then load it in the browser.
  - Write an email to yourself and include the web page as an attachment.
  - Load the web page up in a word processor, such as Microsoft Word or LibreOffice.


7. You've made a spiffy new web page, and you're ready to share it with the world. What do you need to do so that other people can access it?
  - Put the file on a web server.
  - Load the file in your browser.
  - Write an email to everyone in the world and attach the page to it.
  - Upload a .doc file to your favorite social media site.

8. What does HTML stand for?
  - Hyper Text Markup Language.

9. What does HTTP stand for?
  - Hyper Text Transfer Protocol



- Tools For Editing
- HTML (HyperText Markup Language)
  - the language that provides the structure and text of web pages.
- URL (Web Address)


## What's Javascript?
JavaScript is a high-level, interpreted programming language that adds interactivity to web pages. It is one of the three core technologies of the World Wide Web, along with HTML and CSS. JavaScript can be used to create dynamic and interactive web pages, such as games, animations, and forms. It can also be used to access and manipulate the DOM (Document Object Model), which is a representation of the HTML and CSS elements on a web page. JavaScript is a powerful language that can be used to create a wide variety of web applications.

Here are some of the key features of JavaScript:
  - It is a client-side language, which means that it runs in the user's browser.
  - It is a dynamically typed language, which means that you do not need to specify the type of data before you use it.
  - It is an object-oriented language, which means that you can create objects and use them to represent real-world entities.

It is a lightweight language, which means that it does not slow down web pages.
JavaScript is a powerful and versatile language that can be used to create a wide variety of web applications. If you are interested in learning more about JavaScript, there are many resources available online.

## Comments
- JavaScript comments can be used to explain JavaScript code, and to make it more readable.
- JavaScript comments can also be used to prevent execution, when testing alternative code.

```js
// Single Line Comments

/* 
  Muli-Line Commentss
  Add anything here
*/
````

## Declare Variables
- Variables are containers for storing data (storing data values).
- 4 Ways to Declare a JavaScript Variable:
  - Using var
  - Using let
  - Using const
  - Using nothing

```bash
// Declare with var
var x = 5;
var y = 6;
var z = x + y; // z = 11

// Declare with let
let x = 5;
let y = 6;
let z = x + y; // z = 11

/*
  Always declare JavaScript variables with var, let, or const.
  The var keyword is used in all JavaScript code from 1995 to 2015.
  The let and const keywords were added to JavaScript in 2015.
  If you want your code to run in older browsers, you must use var.
*/

/*
  If you want a general rule: always declare variables with const.
  If you think the value of the variable can change, use let.
  In this example, price1, price2, and total, are variables:
*/

// Declare with const
const x = 5;
const y = 6;
let z = x + y; // z = 11

/*
  The two variables price1 and price2 are declared with the const keyword.
  These are constant values and cannot be changed.
  The variable total is declared with the let keyword.
  This is a value that can be changed.
*/

/*
  All JavaScript variables must be identified with unique names.
  These unique names are called identifiers.
  Identifiers can be short names (like x and y) or more descriptive names (age, sum, totalVolume).

  The general rules for constructing names for variables (unique identifiers) are:
  Names can contain letters, digits, underscores, and dollar signs.
  Names must begin with a letter.
  Names can also begin with $ and _ (but we will not use it in this tutorial).
  Names are case sensitive (y and Y are different variables).
  Reserved words (like JavaScript keywords) cannot be used as names.
*/
```