---
title: "TeamCity-Sample Project"
date: "2018-08-02"
categories: 
  - "devops-tools"
---

This blog will help you to configure a TeamCity-Sample Project.

### Creating a project in TeamCity

1. Clicking on the **Create project**link takes us to the **Create New Project** page where we can create our project by providing a suitable name and an optional description for it, as can be seen in the following screenshot:

![](https://cdn-images-1.medium.com/max/800/1*134tmJvYMfEWnLqYnVdPuA.png)

![](https://cdn-images-1.medium.com/max/800/1*0oM_AUir66uh_3tATIe-Cw.png)

## SUBPROJECTS

TeamCity projects can themselves contain other projects. Subprojects are useful for hierarchical display and classification of builds, and also help in easily configuring and sharing similar settings and entities across projects. One use case of subprojects is to have subprojects for each branch/release version of a code base.

The root project is a special project that is the parent of every project and is automatically created. Furthermore, it cannot be deleted or have any build configurations of its own. The main purpose of the root project is to create and maintain settings and entities (such as VCS roots) for use by all projects in the server.

### Adding build configurations

Build configurations in TeamCity are a collection of tasks that make a build, along with the settings needed to describe where the build fetches the source code from, when it runs, and what artifacts it produces.

or our sample project, we will begin by adding a build configuration named `build`. This build configuration will perform build activities such as checking the code for errors and running unit tests and coverage.

![](https://cdn-images-1.medium.com/max/800/1*LjCLlu_Axs6Q8aScCMlKEw.png)

- The **Build number format** setting is used to specify the format in which the build number for each build of this build configuration is to be recorded. The exact format will depend on the type of project and/or organizational/team conventions.
- The **Artifact paths** setting is used to configure the paths from which artifacts have to be uploaded for this build. TeamCity’s artifact management process is such that the generated files and folders in an agent during a build process can be marked as artifacts using this setting. These files are uploaded back to the TeamCity server and exposed via the web interface as artifacts. Any other build configuration that is dependent on this particular build configuration can fetch and use these artifacts as needed. We will come back to configuring and using artifacts in a later section and leave this setting empty for now.
- There are three **Build options** that can be set up as desired.
- **Hanging builds detection** can be enabled if we want TeamCity to detect and stop build configurations that have been running for a long time — longer than their usual runtime — and not providing any messages back to TeamCity.
- The **Status widget** can be enabled to make build configuration information available through various APIs. For example, **Build monitors**can generally access the status of a build configuration only if this setting is enabled. It is recommended to enable both these settings as they are highly useful.
- For the **Limit the number of simultaneously running builds (0 — unlimited)** setting, it is recommended to set a value of `1`. This means that at any point in time, only one instance of this build configuration will run, which is what we require for our CI setup. Setting a positive integer _n_means that _n_ number of instances of this build configuration will run (in _n_ agents.) Setting it to `0` will allow for a potentially unlimited number of instances (limited only by the number of available agents).

Click on **VCS Settings** to move to the next step of creating a build configuration.

Click on the **Create and attach new VCS root** button to go to the VCS creation page. The first step is to choose the VCS type. The choices include **SVN**, **CVS**, **Perforce**, **Git**, and **Mercurial**, among others

The **Push URL** setting is used to specify the URL through which we can push tags from the steps run in the build configuration. It is of use if we have to use a separate fetch URL for checking out and a separate push URL to which we can push tags. Leaving this setting empty will make use of the fetch URL for such push purposes too.

The **Default branch** is the branch that is to be checked and also monitored for changes. By default, it will be `refs/heads/master`, that is, the `master` branch.

![](https://cdn-images-1.medium.com/max/800/1*QRpe2b1snMVp1VrTp0JL-A.png)

When we are building release builds out of a branch, we can use that specific branch in this setting, say `refs/heads/master/1.1.0`. The **Branch specification** setting allows us to define additional branches/refs that need to be monitored for changes. **Use tags as branches** allows tags to be treated as branches in the **Branch specification** settings.

**Username style** defines how a user on TeamCity can link their username to authors of commits in the VCS. The default is **Userid**, which is ideal.

The **Submodules** settings define whether submodules must be checked while cloning/updating the repository. The default, and recommended, setting is to use **Checkout**, which means submodules are checked out and updated when the parent repository is cloned and updated. The other option is to ignore submodules.

Submodules are subrepositories contained within a parent Git repository. Submodules enable one repository to live within another as a subfolder but still be treated as a separate repository of its own. Consider the example of **Build and Release** scripts. They are common to many projects, and it is ideal to have them collocated with each project that needs them. Without duplicating such scripts, the **Build and Release** scripts can be maintained as a separate repository and added as submodules to the project repository that needs it.

![](https://cdn-images-1.medium.com/max/800/1*7IFpf4UJ4np5V1Gwceef_g.png)

![](https://cdn-images-1.medium.com/max/800/1*bW0O0Sj2JDDuvzS99pvnHQ.png)

## INTRODUCING THE BUILD STEPS

Build steps are the individual sequential tasks that are performed within a build configuration.

We will choose the **Command Line** runner as the **Runner type** for the build configuration. This loads the necessary settings, which can be configured as shown in the following screenshot:

For the `build` build configuration, we will be adding two build steps. Firstly, we will echo any statement or you can run any command

echo Nidhi

Next, we will run the unit tests of the project, using the following command:

mvn test

![](https://cdn-images-1.medium.com/max/800/1*X1A8qDp4YHopfI-05Q6LTw.png)

![](https://cdn-images-1.medium.com/max/800/1*QH_iUiY5PZRynNkLWWr3Pg.png)

Finally, you can click on Run and trigger the build.
