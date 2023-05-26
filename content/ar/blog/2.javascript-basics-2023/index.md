---
title: "Javascript Basics In Arabic - 2023"
date: 2023-05-15
draft: false
slug: "javascript-basics-in-arabic-2023"
cascade:
  showReadingTime: true
categories: ['Javascript']
tags: ['js']
---

## ما هى لغة الجافاسكريبت?
JavaScript هي لغة برمجة مفسرة عالية المستوى تضيف التفاعل إلى صفحات الويب. إنها إحدى التقنيات الأساسية الثلاثة لشبكة الويب العالمية ، إلى جانب HTML و CSS. يمكن استخدام JavaScript لإنشاء صفحات ويب ديناميكية وتفاعلية ، مثل الألعاب والرسوم المتحركة والنماذج. يمكن استخدامه أيضًا للوصول إلى DOM (نموذج كائن المستند) ومعالجته ، وهو تمثيل لعناصر HTML و CSS على صفحة الويب. JavaScript هي لغة قوية يمكن استخدامها لإنشاء مجموعة متنوعة من تطبيقات الويب.

فيما يلي بعض الميزات الرئيسية لجافا سكريبت:
   - إنها لغة من جانب العميل ، مما يعني أنها تعمل في متصفح المستخدم.
   - إنها لغة مكتوبة ديناميكيًا ، مما يعني أنك لست بحاجة إلى تحديد نوع البيانات قبل استخدامها.
   - إنها لغة موجهة للكائنات ، مما يعني أنه يمكنك إنشاء كائنات واستخدامها لتمثيل كيانات العالم الحقيقي.
   
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