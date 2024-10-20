---
title: "Encrypt and decrypt files using GPG | gpg4win Kleopatra Tutorial"
date: "2020-11-19"
categories: 
  - "devops-tools"
tags: 
  - "encryption"
---

In this blog, we will explore how can we encrypt and decrypt files using GPG.

GPG (GNU Privacy Guard) is a free encryption software which is compliant with OpenPGP Standard.

We will learn how to use PGP encryption to send encrypted messages to anyone.

### **How to do the setup**

1. Download software from [here](https://www.gpg4win.org/) 
2. Click on the exe file, install it with all default settings
3. Generate the keypair

Once installation is complete, below screen will come 

![](https://cdn-images-1.medium.com/max/800/1*AukUAxjQk8S5hhfCMz3ECA.png)

Click on New Key Pair — you can provide any random values.

Provide the passphrase which will be used later to import or decrypt any file.

![](https://cdn-images-1.medium.com/max/800/1*6pBkfx9Tfnu_MsVdIUchSA.png)

![](https://cdn-images-1.medium.com/max/800/1*T2daDu0_nx-BMh33CUlbrw.png)

![](https://cdn-images-1.medium.com/max/800/1*wJmhj6-zqx2-Do1mRQZK6A.png)

Store the keypair on your machine by selecting an option “**Make a Backup of your keypair”.** This will store two files, one is private key and one is public key.

Private key must not be shared by anyone else. Public Key can be shared with anyone so that they can share the secrets in an encrypted form.

### **How to share secrets**

Now we will see how we can share the secrets with anyone. For ex- My colleague ask me for the database password then how can I share it in a secure way

#### Steps :

1. Import the public key of the user

Click on Import — select the public key of the user which he has provided. It is recommended that we share the public key as an attachment.

![](https://cdn-images-1.medium.com/max/800/1*_UyQia-9Q-2R7k296zWcAQ.png)

Finally, you will see the pop up message “Import is completed successfully"

2\. Copy the secrets

Open notepad- copy the secret — click on tools -> clipboard-> encrypted -> Add recipient ( select user)

![](https://cdn-images-1.medium.com/max/800/1*j4UWbx1IwyA_ra2i818tDg.png)

![](https://cdn-images-1.medium.com/max/800/1*DHM2NaiDT05zjINhzojsMQ.png)

![](https://cdn-images-1.medium.com/max/800/1*I2ZYk5HSwZad7uOC-KuThA.png)

![](https://cdn-images-1.medium.com/max/800/1*aK7kRhRJtICoPzyLBlTrIg.png)

Copy the encrypted message to your notepad and share it with the user.

### **How to open the encrypted text**

Now we will see how we can open the encrypted text send by anyone

1. Provide your public to the user. User has to import your public key
2. User will share the encrypted message
3. Open Kelopatra — Click notepad — paste the encrypted message-> click on Decrypt/verify as highlighted below

![](https://cdn-images-1.medium.com/max/800/1*G6rws97-m5rXOQbCuGZRAw.png)

This will prompt for the passkey and after that you should be able to see the decrypted text

![](https://cdn-images-1.medium.com/max/800/1*6SoARgTceDbJYKU4LfM50A.png)

Congratulations, you have successfully explore how can we encrypt and decrypt files using GPG.
