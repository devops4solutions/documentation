---
title: "Virtual World vs Container World"
date: "2018-09-04"
categories: 
  - "docker"
---

In this blog, we will explore the Virtual World vs Container World

# Example

To help me explain the differences between the virtual world and the container world, I would like to tell you a story about Mary.

Mary is a student looking for accommodation. In the virtual world, Mary is offered a series of castles.

- large wardrobe spaces but nowhere to sleep
- lots of bedrooms, bathrooms, and even have swimming pools
- huge driveways but nowhere to store books

 Each castle is very expensive. Mary simply doesn’t have the resources to afford any of these. She says -

- “I don’t have many clothes or fancy ballgowns, but I do have lots of books to store.
-  I don’t know how to swim and I only have one car.

 The accommodation officer says,

- “Ah, you only have one car at the moment, but you may have more in the future
- you might learn to swim, and gain lots of fancy clothes.
- You may not need these things right now, but you may need these things in the future, and this gives you options.

 Mary says, “I simply don’t have the resources or the desire to buy something that I probably won’t use.” And, with that, she leaves the virtual world and enters the accommodation office in the container world.

Unlike the virtual world, Mary is asked what her minimum requirements are. She say I need

- a bedroom
- a bathroom
- a kitchen

 and Mary is offered exactly that: three rooms — or, in this case, containers — each suited to her needs. One has a bedroom, the other is a bathroom, and, lastly, a kitchen.

“Great,” Mary says, “but what happens when I have friends over? Where are they going to sleep?”

 Accommodations officer says,

- we can either duplicate your current room or create you a new one, but you’ll only pay for what you need.
- Also, if you wanted to move to another building, we can replicate each room exactly as you have left them; or, if you don’t want a particular room, we can remove that for you.

You see, virtualization requires upfront costs and resources. Some of these may not be needed, or may be too little for the task at hand. Containers are lightweight. They perform specific functions or tasks, and can be easily replicated for scale.

![](https://cdn-images-1.medium.com/max/800/1*Gg6F9SnZ-9IyUKCBsq9m4A.png)

**Virtual Machine Setup**

- A host machine. This could be a server, or it could even be your laptop.
- The host machine has a hypervisor, which is a virtual machine monitor, or VMM for short. The VMM allows us to create virtual machines that have virtual hardware and software. A virtual machine will have its own guest operating system, and, of course, this is where the apps will live.
- A major difference between the two setups is that with virtualization, we rely on the hypervisor to manage and create our virtual machines. This, of course, creates a virtual set of resources, which are siloed per environment, and each virtual machine requires its own guest operating system, libraries, and, finally, the application or applications themselves.

**Container Setup**

- A host machine. This could be a server, or it could even be your laptop.
- We have our Docker Engine that manages the containers. However, in a container setup, we use the Docker Engine to create and manage containers via a set of daemons connected to the host kernel. It is important to remember that a container is ephemeral in nature. In production, many of the same containers will coexist and be removed or replaced without noticeable disturbance on the service.
- But, what if you wanted to isolate Docker instances even further? Perhaps you may be tempted to have Docker running in virtual machines. Whilst this is possible, it is highly recommended to use the tool called Docker Machine. This tool allows you to install and access the Docker Engine on virtual hosts. These virtual hosts will be tailored specifically for Docker and, furthermore, can be controlled remotely.

![](https://cdn-images-1.medium.com/max/800/1*eEjazEsXx4RSmpEdvCrdkw.png)
