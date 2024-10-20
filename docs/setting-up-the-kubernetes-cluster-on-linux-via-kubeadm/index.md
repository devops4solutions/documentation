---
title: "Setting up the Kubernetes cluster on Linux via kubeadm"
date: "2018-08-21"
categories: 
  - "devops-tools"
---

In this blog, we are going to show how to create a Kubernetes cluster along with kubeadm on Linux servers.

Kubeadm is a command-line tool that simplifies the procedure of creating and managing a Kubernetes cluster. Kubeadm leverages the fast deployment feature of Docker, running the system services of the Kubernetes master and the etcd server as containers. When triggered by the kubeadm command, the container services will contact kubelet on the Kubernetes node directly; kubeadm also checks whether every component is healthy.

Prerequisite:

1. 1. Docker should be installed

1. 1. Network Ports should be available

1. Network tool packages shoud be installed.

**sudo yum install ethtool
sudo yum install ebtables**

![](https://cdn-images-1.medium.com/max/800/1*ZmbogovLUSnOve6fsqgEDw.png)

- The Linux command, netstat, can help to check if the port is in use or not:

**// list every listening port**
**$ sudo netstat -tulpn | grep LISTEN**

**Install Kubernetes Package**

1. Add new repository

**$ sudo vi /etc/yum.repos.d/kubernetes.repo**
**\[kubernetes\]**
**name=Kubernetes**
**baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86\_64**
**enabled=1**
**gpgcheck=1**
**repo\_gpgcheck=1**
**gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg**
 **https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg**

2\. Now, we are ready to pull the packages from the Kubernetes source base via the yum command:

**/ on Kubernetes master
sudo yum install -y kubelet kubeadm kubectl
// on Kubernetes node**
**$ sudo yum install -y kubelet**

![](https://cdn-images-1.medium.com/max/800/1*g51lC9ILQr77-uxqIu0zpQ.png)

3\. Check the version

kubectl version

![](https://cdn-images-1.medium.com/max/800/1*SRbIHDg_qt7dDKZt00qtWg.png)

kubelet — version

![](https://cdn-images-1.medium.com/max/800/1*dxMMWDD34kwsfzGxznBbAw.png)

System Configuration

**sudo setenforce 0
/ enable the parameters by setting them to 1**
**$ sudo bash -c 'echo "net.bridge.bridge-nf-call-ip6tables = 1" > /etc/sysctl.d/k8s.conf'
$ sudo bash -c 'echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.d/k8s.conf'
// reload the configuration
$ sudo sysctl --system**

![](https://cdn-images-1.medium.com/max/800/1*rFxGQIoE2xBUo7Va1FOomA.png)

Now start the service

**sudo systemctl enable kubelet && sudo systemctl start kubelet**

While checking the status of kubelet, you may be worried to see the status displaying activating (auto-restart); and you may get further frustrated to see the detail logs by the journalctl command, as follows:

error: unable to load client CA file /etc/kubernetes/pki/ca.crt: open /etc/kubernetes/pki/ca.crt: no such file or directory

Don’t worry. kubeadm takes care of creating the certificate authorities file. It is defined in the service configuration file, /etc/systemd/system/kubelet.service.d/10-kubeadm.conf by argument KUBELET\_AUTHZ\_ARGS. The kubelet service won’t be a healthy without this file, so keep trying to restart the daemon by itself.

You might see below screen

![](https://cdn-images-1.medium.com/max/800/1*_-TBCYFYCaGzlrBxmh_GVw.png)

No start the kubeadm

**sudo kubeadm init**

![](https://cdn-images-1.medium.com/max/800/1*HJmbM_-o5c9RoOyjALpSuA.png)

And you will see the sentence Your Kubernetes master has initialized successfully! showing on the screen. Congratulations! You are almost done! Just follow the information about the user environment setup below the greeting message:

**$ mkdir -p $HOME/.kube**
**$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config**
**$ sudo chown $(id -u):$(id -g) $HOME/.kube/config** 

Now check the version

**// Your kubectl command works great now**
**$ kubectl version**

![](https://cdn-images-1.medium.com/max/800/1*mYndImgHvKOfcmz7Nvf2CQ.png)

Now check the status, all errors will gone now

**$ sudo systemctl status kubelet**

![](https://cdn-images-1.medium.com/max/800/1*6hkKz5BOxT6sC5ko3YOfKA.png)

### Network configurations for containers

After the master of the cluster is ready to handle jobs and the services are running, for the purpose of making containers accessible to each other through networking, we need to set up the network for container communication. It is even more important initially while building up a Kubernetes cluster with kubeadm, since the master daemons are all running as containers. kubeadm supports the CNI ([https://github.com/containernetworking/cni](https://github.com/containernetworking/cni)). We are going to attach the CNI via a Kubernetes network add-on.

There are many third-party CNI solutions that supply secured and reliable container network environments. Calico ([https://www.projectcalico.org](https://www.projectcalico.org/)), one CNI provide stable container networking. Calico is light and simple, but still well implemented by the CNI standard and integrated with Kubernetes:

**$ kubectl apply -f** [**https://docs.projectcalico.org/v2.6/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml**](https://docs.projectcalico.org/v2.6/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml)

Here, whatever your host OS is, the command kubectl can fire any sub command for utilizing resources and managing systems. We use kubectl to apply the configuration of Calico to our new-born Kubernetes.

Please reach out to devops4solutions team to provide professional service to setup Kubernetes cluster on Linux via Kubeadm.
