---
title: "SSH Key Concepts and SSH Tunnel"
date: "2018-09-22"
categories: 
  - "devops-tools"
tags: 
  - "ssh"
  - "ssh-tunnel"
---

In this tutorial we will explore the SSH Key Concepts and SSH Tunnel.

SSH use public/private key methods for authorization and authentication

### **What is Public Key**

public key is like a lock. It’s not actually a key. it’s a lock you can make lots of copies of and distribute wherever you want. For example, if  I need an access to any of the machine then I need to put my lock on that ssh account on another machine.

I will generate ssh key on my machine and then copy that public key  to ‘authorized\_keys’ in the ~/.ssh folder.

And now to access that machine, I make a putty connection and use my private key in the authorization.

### **What is Private Key**

Private key is the actual key. This is what you use to open the lock that is stored on the other machine. Just like a regular key you keep it secret, safe, and out of the wrong hands.

### Route Web Traffic Securely Without a VPN Using a SOCKS Tunnel

Sometimes you're on a network that's insecure and you need to access a website. You want to make sure no one in the middle is watching the traffic.

One solution is a [VPN](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-14-04), but many VPNs require special client software on your machine.

So another alternative is a SOCKS 5 proxy tunnel to secure your web browsing.

A SOCKS proxy is basically an SSH tunnel in which specific applications forward their traffic down the tunnel to the server, and then on the server end, the proxy forwards the traffic out to the general Internet. Unlike a VPN, a SOCKS proxy has to be configured on an app by app basis on the client machine, but can be set up without any specialty client agents.

**How to Setup a SSH Tunnel**

1. Putty for Windows
2. Firefox

Complete the following steps to set up the tunnel:

1. From the **Session** section, add the **Host Name (or IP address)** of your server, and the SSH **Port**(typically 22)![Putty Sessions](https://assets.digitalocean.com/articles/socks5/wXDz8J7.png)
2. On the left, navigate to: **Connection > SSH > Tunnels**
3. Enter any **Source port** number between 1025-65536. In this example we've used port 1337![Putty Connection>SSH>Tunnel](https://assets.digitalocean.com/articles/socks5/ZLPgf4V.png)
4. Select the **Dynamic** radio button
5. Click the **Add** button
6. Go back to **Session** on the left
7. Add a name under **Saved Sessions** and click the **Save** button
8. Now click the **Open** button to make the connection
9. Enter your sudo username and server password to log in

You can minimize the PuTTY window now, but don't close it. Your SSH connection should be open.

## Configuring Firefox to Use the Tunnel

1. Open browser -> Options -> Settings -> Set Manual Proxy configuration

![](https://cdn-images-1.medium.com/max/800/1*bXVjnOq5QPfx7E_1Id3M8w.png)

### Deploy a Stack
