---
title: "SonarQube Installation on Windows"
date: "2020-08-16"
categories: 
  - "sonarqube"
---

In this blog, we will do the SonarQube Installation on Windows. Quick and easy steps for download and SonarQube installation on Windows to automate code inspection

Read out my article  [**What is sonarqube**](https://devops4solutions.com/what-is-sonarqube/) to gain basic knowledge of the tool.

## **Pre-requisite:**

- Java 1.8 or above  as per the version of the sonarqube (Make sure to install it on your system)

## Download Sonarqube

1\. Download [SonarQube](https://www.sonarqube.org/downloads/):  In this article, we will install 8.4.1 version of sonarqube  

         \* Download the latest stable version and extract the .zip on to the local system.

2\. Extract the sonarqube binaries and navigate to the directly and run the below command

 D:\\DevOps\\sonarqube-6.7.3\\bin\\windows-x86-64  
StartSonar.bat

3\. If you get the java error then make sure JAVA\_HOME is set properly or manually update the wrapper.conf file which is present under conf folder of sonarqube installation directory to point to your java 

wrapper.java.command=C:\\Program Files\\Java\\jdk-14.0.2\\bin\\java

![](https://devops4solutions.com/wp-content/uploads/2018/06/Capture-3-300x117.png)

\[caption id="" align="alignnone" width="1200"\]![](https://cdn-images-1.medium.com/max/1100/1*CaCe4yRIy9cgQ2jqDWf0YA.png) Running SonarQube on command prompt\[/caption\]

4\. Once the SonarQube is up, go to the browser : [http://localhost:9000](http://localhost:9000) to access SonarQube dashboard.

![sonarqube dashboard](https://cdn-images-1.medium.com/max/800/1*MBs47Vi80NQ3Jar_1ru2xA.png)

5\. Login with default credentials (admin both as userId and Password).

6\. Go to Administration -> Security ->Generate token (copy and save  the token for future use, e.g. while integrating with Jenkins).

##### **Background**

_How to evaluate Quality of code?_

It is quite facile to evaluate other products and professions, viz quality of a dairy product, cosmetic product or Doctors( via number of successful operations) etc. In the process of evaluating quality , there exists certain enumerated standards, depending on which one can decide the grade of that product/service. More the checked list of standards followed is directly proportional to the increasing quality of the product/service.

Example : Quality characteristics/attributes for a sunscreen product

- Amount of coverage (e.g. spf 30, spf 40 etc)
- Skin Biocompatibilty
- Sensitive skin suitability
- Physical barrier
- Chemical barrier
- Suitability to various skin types (oily, dry & combination skin etc)

_"More your product sticks to the standards, more is its quality and acceptability by the customers."_

Now, assume the company has created a product which satisfies only 4 characteristics and it reaches the market. May be after purchasing, it is possible that customers might not be satisfied with the quality or attributes that it offers. And because of the negative feedback, the company binds to invest again on research to adapt to the best standards available.

Let's switch the sunscreen for a software product, company with the developers and customers with the clients/end users. Perception of both  developers and end users related to product quality is different.

What can be software characteristics/attributes -

1. Specific convention to be followed while coding.
2. No duplicates code.
3. Less complicated code logic.
4. Established best practices to be followed.
5. Avoid bad practices.
6. Code with good documentation & comments.
7. Unit tests associated with code.
8. Good design and architectural principles followed by code.

Once the characteristics are pen down, now the major question is how to impose it to automation ???

_Here enters "SonarQube" for static code analysis._

## SonarQube for automated analysis

##### **What is SonarQube?**

As defined by Wiki, SonarQube is an open source platform developed by SonarSource for continuous inspection of code quality. It is written in JAVA and supports 20+ programming languages.

\[caption id="attachment\_595" align="alignnone" width="540"\][![](https://devops4solutions.com/wp-content/uploads/2018/06/Capture-2-300x145.png)](https://www.sonarqube.org/) SonarQube Home Page\[/caption\]

###### Features:

1. Not just tells what is wrong, also aids you to rectify via Quality Management tool.
2. Covers a greater stretch as far as definition of Quality is concerned (as proudly flaunt by its developers - Seven Axes of Quality), as compared to its competitors, whom major emphasis is on bugs & complexity.
3. Its focus is not only just constraint to potential bugs or code conventions, but also extends to span coding standards, duplications, test coverage, unit testing, code complexity, API documentation, architecture, comments, bugs along with providing details in a dashboard is cherry on the top.
4. It provides metrics, tracking of which can help you to take right decision.
5. It provides various features to support Continuous Inspection practice, viz. Quality Gates, Focus on leak, highlighting hotspots, branch analysis, analyze pull requests, visualizing the history of project.

##### **Why SonarQube?**

Even if your company has considerable number of developers, who can persistently peer review each other's code, the result would be still susceptible to errors and false positive outcomes. Thus doing static code analysis manually is extremely time consuming when dealing with enormous amount of code.

Thankfully automated analysis is available to save the situation. It is comparatively faster & facile method to consistently check the code for errors. It blends a large, predefined set of common & less-common errors with intelligent algorithms and techniques, in order to systematically and cost-effectively track down bugs, potential errors or poor coding practices in a  matter of seconds, while it would take a human hours or days.

SonarQube -->  Collects & analyze source code --> measure quality ---> provide reports for your project.

**Conclusion:**

Everything that affects our code base, from minor styling details to critical design errors, is inspected and evaluated by SonarQube, thereby enabling developers to access and track code analysis data ranging from styling errors, potential bugs, and code defects to design inefficiencies, code duplication, lack of test coverage, and excess complexity. The Sonar platform analyzes source code from different aspects and hence it drills down to your code layer by layer, moving from the module level down to the class level. At each level, SonarQube produces metric values and statistics, revealing problematic areas in the source that require inspection or improvement.

_Congratulations!! You have successfully completed  SonarQube installation on your system and you are ready to integrate it with CI server to analyse your code._

**References:**

- https://www.sonarqube.org/features/clean-code/
