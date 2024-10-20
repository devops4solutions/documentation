---
title: "What is Packer"
date: "2018-08-30"
categories: 
  - "devops-tools"
---

A machine image is a static unit that contains a preconfigured operating system and installed software. You can use images to clone or create new hosts. Images help speed up the process of building and deploying new infrastructure. Images come in many formats, specific to various platforms and deployment environments.

Previously, to built the servers we stacked with four PCs, a CD burner, and an assortment of cables and CDs, around the data center. We plugged directly into freshly racked servers and loaded previously built images, usually called “golden images,” onto the new servers manually and slowly. The process was slow and painful. It also created hard-to-manage servers.

Every image was a snapshot, a point-in-time collection of packages and configuration. Each server was generally fixed at that point in time. This meant your images quickly became out of date and potentially vulnerable to security issues in older packages. Mass updates — for example, package updates — were possible but tricky and, because the “golden image” model predated configuration management tools like [Puppet](https://puppet.com/), mass configuration changes were complex and error prone.

**What is Packer**

Packer is a free and open-source image-building tool, written in Go. It allows you to create identical machine images, potentially for multiple target platforms, from a single configuration source. Packer supports Linux, Microsoft Windows, and Mac OS X, as well as other operating systems, and has support for a wide variety of image formats, deployment targets, and integrations with other tools.

Packer is fast, relatively quick to learn, and easy to automate. When used in combination with configuration management tools, it can create complex and fully functional images with software preinstalled and preconfigured.

**Why Packer?**

Building images is tedious. It’s also often manual and error prone. Packer can automate the creation of images and integrate well with your existing infrastructure tools. Packer allows you to create pipelines for building and deploying images, which in turn allows you to produce consistent, repeatable images.

**Packer Use Cases**

1. Continuous Delivery

Packer integrates well with existing infrastructure tools. It can be slotted into a deployment pipeline and used to ensure the images you build on, test, and deploy are correct and up to date. We use a Continuous Integration/Continuous Deployment (CI/CD) approach to deploy new Amazon EC2 images. Packer builds Amazon Machine Images (AMIs) using Puppet, Terraform uses those AMIs when hosts and services are built, and Puppet runs again (and periodically) to provide final configuration and to keep our hosts correctly configured.

This means that if we need a new host or have to replace a malfunctioning host, the process is fast and consistent. Our infrastructure then becomes disposable, replaceable, and repeatable.

#### Environmental consistency

Do you have a large, complex infrastructure, with numerous environments covering development, testing, staging, and production? Packer is ideal for standardizing images across those environments. As it supports numerous target platforms, you can build standard images for all sorts of fabrics. You can also ensure that consistent configuration for things like patching, time, networking, security, and compliance are maintained across environments. For example, an infrastructure or a security team can use Packer to build images that are then shared with other groups to provide baseline builds that force cross-organizational standards.
