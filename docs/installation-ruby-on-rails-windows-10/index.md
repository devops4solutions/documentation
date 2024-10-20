---
title: "Installation of Ruby on Rails on Windows"
date: "2018-08-16"
categories: 
  - "devops-tools"
tags: 
  - "ruby"
  - "ruby-on-rails"
---

With this blog, we will learn Installation of Ruby on Rails (Rails) on Windows 10 in detailed and an easy step-by-step process.

Pre-requisite:

1. Ruby
2. Ruby gems
3. Git
4. IDE (Notepad can also be used)

Let's start with installation of Ruby.

#### Installation of Ruby on Windows

###### Step 1. Check whether ruby is installed or not.

C:\\Users\\user> ruby -v

![](https://cdn-images-1.medium.com/max/1000/1*c_rAUk3muq-QLU0GPukuQw.png)

###### Step 2. To install Ruby on Windows, go to this link [https://rubyinstaller.org/](https://rubyinstaller.org/)

![](https://cdn-images-1.medium.com/max/1000/1*vDNJ0-ptsPFR1wdyRdVRWA.png)

###### Step 3.  Click on Download. It will navigate you to the download window.

![](https://cdn-images-1.medium.com/max/1000/1*vT_w8W8XSnwUlRpIzManDA.png)

Download the 64-bit .exe file (if your system is 32-bit then download x86 version and if using older forms like Vista then it is recommended to download Ruby 2.19)

###### Step 4. Run the rubyinstaller.exe file

![](https://cdn-images-1.medium.com/max/1000/1*sxrZ5FqmLNEqFnn7_811zg.png)

![](https://cdn-images-1.medium.com/max/1000/1*xbksknUxOXILoJl66rGpJQ.png)

\*Make sure all the three boxes are checked.

![](https://cdn-images-1.medium.com/max/1000/1*kngA3rj76NMdfO8flUimlg.png)

![](https://cdn-images-1.medium.com/max/1000/1*SyExPOAK6id0NJK-Zi9Pkw.png)

Once the package will install, the ruby gems will be installed along with it.

**Ruby gems: It is a package manager for Ruby and provides a standard format to distribute Ruby programs and libraries.** 

Each gem has a name, version, and platform. For example, the rake gem has a 0.8.7 version. Rake’s platform is ruby, which means it works on any platform Ruby runs on.

 

![](https://cdn-images-1.medium.com/max/1000/1*AnhEyquPXhPZofczy5B0_w.png)

Once you will click ‘Finish’, following command prompt will be displayed.

![](https://cdn-images-1.medium.com/max/1000/1*H1K-cGwBSZxzKfdxHPV0Rw.png)

- Note: They are just command prompt alternatives. You can ignore them and close the window to continue using your command prompt.

###### Step 5. Go to Command prompt and check the ruby installation.

![](https://cdn-images-1.medium.com/max/1000/1*2UNr5PMexFvjGSetEuyXNg.png)

Oopss.. We are required to set the path of Ruby bin folder in our Windows environment variables.

###### Step 6. Set the PATH :

a). Go to Sart → Type System

![](https://cdn-images-1.medium.com/max/1000/1*iRqSuF275IEHozV3fhcTYA.png)

b). Click ‘Advanced system settings’

![](https://cdn-images-1.medium.com/max/1000/1*N41PMLzbK2x4gM87hBHWxw.png)

c). Click on ‘Environment Variables’

![](https://cdn-images-1.medium.com/max/1000/1*jEQ_c6TazJ0FP09X3dex6A.png)

In ‘System variables’ section,scroll down and select ‘Path’ → Click ‘Edit’

![](https://cdn-images-1.medium.com/max/1000/1*b8fgEr5gq9vCITAkaBDMTQ.png)

d). Click on ‘New’, and provide the path to the bin folder of Ruby and click ‘OK’.

![](https://cdn-images-1.medium.com/max/1000/1*5B5cddAqXZcfKYqn9gXAiQ.png)

###### Step 7. Now check the Ruby version again from command prompt.

![](https://cdn-images-1.medium.com/max/1000/1*sF8jc9OR7MadkynNVCmVSA.png)

Voillaa!! We have installed Ruby successfully on Windows.

#### Installation of Ruby on Rails on Windows

_**Install Rails using Rubygems**_− With Rubygems loaded, you can install all of Rails and its dependencies using the following command through the command line −

C:\\Users\\user> gem install rails

You might get this security alert. Click on ‘Allow access’.

![](https://cdn-images-1.medium.com/max/1100/1*inyauFeWedNIi-psQZnK8g.png)

![](https://cdn-images-1.medium.com/max/1100/1*jdgXtopZok_y7orfVEjlIA.png)

Note: It will take some time to install all dependencies. Therefore, while installing gem dependencies, a persistent internet connection is required.

_**Check Rails version:**_

C:\\Users\\user> rails -v

![](https://cdn-images-1.medium.com/max/1100/1*0ozI1O9tKQuBAkZsOLViGQ.png)

_**Congratulations!! We are now on RAILS 5.2.1 on Windows.**_

_**To keep rails updated:**_

C:\\Users\\user> gem update rails
