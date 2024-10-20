---
title: "Calling Child Method from Parent Component in Lightning Components"
date: "2018-08-26"
categories: 
  - "devops-tools"
tags: 
  - "auramethod"
  - "calling-child-method-from-parent"
  - "components"
  - "lightning"
  - "salesforce"
---

Hello All,

We know that we can interact with parent component from child component by firing the events. you may encounter the scenarios  where you need to call child component’s method from Parent Component. In this case, we use <aura:method>.

To achieve this, follow these steps:-

1. Define aura:id of the child component in parent.
2. Define <aura:method> in child component and it will link to method of child controller.
3. Define a lightning:button on parent component and on click of this button get the reference of the child component as mentioned in step 1 and call aura:method as mentioned in step 2.

Pretty easy!!! Right?

![](https://devops4solutions.com/wp-content/uploads/2018/08/app.jpg)

Let’s see some code in action for this demo:

Child Component (.cmp file)

\[snippet slug=child-component-115-2-1 lang=abap\]

Child Component (controller.js file)

\[snippet slug=child-component-115-2-2 lang=abap\]

Parent Component (.cmp file)

\[snippet slug=parent-component-1115-2-1 lang=abap\]

Parent Component (controller.js file)

\[snippet slug=parent-component-115-2-4 lang=abap\]

Application (.app File)

\[snippet slug=application-container-115-4-5 lang=abap\]
