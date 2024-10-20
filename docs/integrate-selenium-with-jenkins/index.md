---
title: "Integrate Selenium with Jenkins"
date: "2018-06-13"
categories: 
  - "jenkins"
tags: 
  - "jenkins"
  - "maven"
  - "selenium"
---

# How to Integrate Selenium with Jenkins

This blog will help you to Integrate Selenium with Jenkins. This will help you to run the selenium scripts using Jenkins automatically or as per your need.

## Requirement

1. Download the sample code from github
2. Maven Installation
3. Setup with eclipse

### Download the sample code from github

1. Clone the repository from [here](https://github.com/nidhigupta12/SeleniumScript.git)
2. cd to the location
3. run the mvn test on your machine.

### Jenkins Configuration

1. Create a freestyle job, in scm — use the link mentioned above -> in build section -> add clean test -> save ->trigger

### Setup with eclipse

See the below steps if you want to know how to configure a sample project in eclipse to run selenium scripts.

1. Download eclipse from [here](http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/oxygen/3/eclipse-jee-oxygen-3-win32-x86_64.zip&mirror_id=1249)
2. Create a folder on your machine C://Workspace
3. Launch eclipse.exe ( Note java8 is required)
4. Create a new Maven Project

![](https://cdn-images-1.medium.com/max/800/1*9DhzaQgLtQEcMDpawTcbiA.png)

6\. Click Next and select create a sample project

![](https://cdn-images-1.medium.com/max/800/1*2uu9-cJVliwo9WQqEKAi1w.png)

![](https://cdn-images-1.medium.com/max/800/1*XTkTUdi8TUMW7e0C8qz7qA.png)

7\. Project created successfully.

8\. Add TestNG Plugin in Eclipse.

#### Install from update site

- Select _Help / Install New Software…_
- Enter the update site URL in “Work with:” field:
- Update site for release: [http://beust.com/eclipse](http://beust.com/eclipse).
- Or, Update site for beta: [http://beust.org/eclipse-beta](http://beust.org/eclipse-beta) , use it if you want to experiment with the new features or verify the bug fixes, and please [report back if you encounter any issues](https://github.com/cbeust/testng-eclipse/issues).
- **Help → Install new software → Here uncheck “Contact all update sites during install to find required software”**
- Make sure the check box next to URL is checked and click _Next_.
- Eclipse will then guide you through the process.

10\. Create a new TestNg Class

![](https://cdn-images-1.medium.com/max/800/1*QJknSlu-3jv7gAez2xjBnQ.png)

NewTest.java

package example;

import org.openqa.selenium.By;

import org.openqa.selenium.WebDriver;

import org.openqa.selenium.firefox.FirefoxDriver;

import org.testng.Assert;

import org.testng.annotations.Test;

import org.testng.annotations.BeforeTest;

import org.testng.annotations.AfterTest;

public class NewTest {

private WebDriver driver;

@Test

public void testEasy() {

driver.get(“http://demo.guru99.com/test/guru99home/");

String title = driver.getTitle();

Assert.assertTrue(title.contains(“Demo Guru99 Page”));

}

@BeforeTest

public void beforeTest() {

driver = new FirefoxDriver();

}

@AfterTest

public void afterTest() {

driver.quit();

}

}

Right-click on the WebdriverTest and select **TestNG** | **Convert to TestNG**. Eclipse will create testng.xml which says that you need to run only one test with the name **NewTest**as shown in the following screenshot:

![](https://cdn-images-1.medium.com/max/800/1*7ynxZ8Jl_8sh4TVTMe8kBQ.png)

Now you need to run test through this **testng.xml.**

So, go to the **Run Configurations** and create a new launch **TestNG**, select the project and field **Suite** as **testng.xml** and click Run

![](https://cdn-images-1.medium.com/max/800/1*T1sE0RIvklKoCSIdO5i9bQ.png)

Build finished successfully

Now run as maven test.
