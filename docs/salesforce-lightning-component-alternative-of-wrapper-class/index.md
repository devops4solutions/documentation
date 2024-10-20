---
title: "Salesforce Lightning Component- Alternative of Wrapper Class"
date: "2018-08-15"
categories: 
  - "devops-tools"
tags: 
  - "best-practices"
  - "lightning"
  - "lightning-components"
  - "salesforce"
  - "wrapper-class"
---

Hello All,

In this post, we will talk about the alternative of using wrapper class  while returning the data from the Apex Class.

In Many scenario,  we need to return the data from more than one object, in that situation we generally approach for the wrapper class.

Lets take a scenario:

Suppose, we have a lightning component where user enter the search text and on the basis of that we need to display records from both Account and Contact object. So what should be the return type from server? Any guesses ???

![](https://devops4solutions.com/wp-content/uploads/2018/08/searchRecords-300x169.png)

The first approach which comes up in our mind is wrapper class.

\[snippet slug=wrapper-class-approach lang=abap\]

Though it solves our purpose but on the other hand it comes with the headache of maintenance of code and we have to write the test class as well. So what we can do in these scenarios????

In these scenarios we can use "List<object>" as a return type and bingo !!! we don't need to write extra wrapper class and its test class.

\[snippet slug=object-approach lang=abap\]

Here is the complete code of the search component.

Search.cmp

\[snippet slug=tutorial\_search-cmp lang=abap\]

searchController.js

\[snippet slug=searchcontroller-js lang=abap\]

Apex Class

\[snippet slug=searchcontroller lang=abap\]

create a new lightning app and keep the component in it.

\[snippet slug=appcontainer lang=abap\]
