---
title: "Redirect a domain to another domain using Amazon Route 53"
date: "2021-05-20"
categories: 
  - "aws"
---

In this blog, we will explore how to redirect a domain to another domain using Amazon Route 53.

Checkout my [Youtube](https://youtu.be/DyU5rDQ8eKE) video on this blog.

### **Requirement**

- You have a domain `abc.com` and DNS is configured on Route 53
- You have another domain `abc.ca` which needs to redirect to `abc.com`

### **Solution**

For domain forwarding, you need to follow the below steps

1. Create S3 bucket with name `abc.ca` 
2. Enable the static website hosting with redirect option
3. Create `A` record in Route53 for hosted zone `abc.ca` which will point to S3 bucket endpoint

**NOTE: If you need** `**www.abc.ca**` **also redirects to** `**abc.com**` **then do the same steps as you do for** `**abc.ca**`

#### **Create S3 bucket**

![](https://cdn-images-1.medium.com/max/1600/1*tZdaHzpbGm-FORUmYJJv4A.png)

![](https://cdn-images-1.medium.com/max/1600/1*GVjyt_83A9mJuVyjgGwm3w.png)

Click on Create bucket

Now enable state website hosting

Click on bucket -> Properties -> Static website hosting

![](https://cdn-images-1.medium.com/max/1600/1*RA6yDX6DChrxaehpZxP_4g.png)

Select Redirect request -> provide your website url

![](https://cdn-images-1.medium.com/max/1600/1*cH1ZQCUKLWoBjHZ6FPpi7A.png)

![](https://cdn-images-1.medium.com/max/1600/1*nKYvbDj_Em-Lh7bu6uV5rg.png)

You can click on the bucket endpoint it should redirect to your website.

Once everything looks good, you can create a Route53 configuration

#### **Configure Route53**

1. Create a hosted zone for your website if not exist 
2. Create a new `A` record and route traffic to S3 endpoint using Alias

![](https://cdn-images-1.medium.com/max/1600/1*W1QfItwDm6q4Tm97Byj5cg.png)

Now you can try accessing `abc.ca` and this will redirect to `abc.com` automatically.

If you have **SSL** applied on `abc.com` then you don’t need any separate SSL for `abc.ca` 

This is how we do the redirection when we need to do at the domain level.

**Why we can’t use CNAME**

If you will try to create a `CNAME` record then you will see the below error

Bad request.  
(InvalidChangeBatch 400: RRSet of type CNAME with DNS name abc.ca. is not permitted at apex in zone abc.ca.)

![](https://cdn-images-1.medium.com/max/1600/1*yz5PIN5C3CQ5C0rz7G38Mg.png)

This process will work for other redirection when your domain is same like 

- `www.abc.ca` / `test.abc.ca` to `abc.ca` you don’t need a S3 bucket
- For these types, you can create a `CNAME` record in route 53 and that will work.
