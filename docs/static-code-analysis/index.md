---
title: "Static Code Analysis"
date: "2018-06-18"
categories: 
  - "sonarqube"
---

#### **Introduction:**

From ‘good to follow’, to now treated as an ‘indispensable requirement’, software code quality & security has seen crest and trough in last few years. Now, it has become an inevitable excerpt of software development life cycle. Almost all organizations have made it a pre-requisite to employ static code analysis test (along with penetration & security test) before deploying code to production stage. Within a development team, it is imperative to receive a fair and unbigoted idea about the quality of the code that is being checked-in to the source code repository, especially to notify everyone regarding any error prone check in.

“_Every code is subjected to errors. Every software has bugs. If you don’t discover them, your users will_”

You must be an active user of instagram, just imagine what if you cant apply filter or crop your selfie to fit to your #instastory due to an error in the code?

That is where static code analysis invades. It runs on the source code, analyzes the code & publishes errors to the developers.

#### Background:

**_Source code analysis_** : is the automated testing of source code for debugging purpose, before it is dispatched for distribution. Source code analysis can be of two types : Static & dynamic analysis.

_**Static code analysis**_ is analyzing the source code without executing them (non-runtime environment), in order to find out potential vulnerabilities, bugs & security threats, thereby ensuring conformance to coding guidelines and effectively displaying them to the developers. The typical example is a compiler which finds lexical, syntactic and even some semantic errors.

After static analysis, _**dynamic code analysis**_ is testing the code while it is being executed in a real or virtual environment. It effectively finds subtle defects or vulnerabilities, as it also inspects the code when it interacts with other databases, servers & services.

In order to attain maximum level of test coverage, it is always recommended to combine both static & dynamic analysis. Together they are referred to as ‘glass-box testing’, as when combined they can completely see/peek inside the ‘box’/ ‘source code’.

## Static Code Analysis

Static code analysis is a collection of algorithms and techniques used to analyze source code in order to automatically find potential errors or poor coding practices.  It is commonly referred as "white-box" testing, as it looks as applications in non-runtime environments.

Let's dig into the benefits & the tasks solved by static code analysis:

1. Identifies potential software quality issues during development phase before the software reaches production.
2. Detection of the excerpts in code that needs re-factoring.
3. Recommendations on code formatting. Some static analyzers allow you to check if the source code corresponds to the code formatting standard accepted in your company.
4. Implements metrics computation. Software metrics are a measure that helps to  get a numerical value of some property of software or its specifications.
5.  Improves communication in the development team and aids training developers to produce high-quality code.

There are many static code analysis tools available. However, Checkstyle (Main Focus : Coding Conventions), PMD (Main focus: Bad practices), and FindBugs (Main focus : Potential Bugs)HP Fortify (Main focus : Security Vulnerabilities), SonarQube are well-known and used in most of the projects.

#### Conclusion:

Static Code Analysis is not just useful but also imperative for software to work and for developers to understand their software. It simplifies the process of searching for bugs and errors, by pointing you right to them and helps identify issues, so you can rectify them. Thus it is time-saving and high-quality code producing practice, which This in return is not only time-saving but it also helps to produce high-quality code, thus killing two birds with one stone,  making both end-users and programmers happy!!

 

###### References:

- https://www.codacy.com
- http://javarevisited.blogspot.com
- https://dzone.com
- https://www.red-gate.com
- https://medium.com
