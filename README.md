# MultiNodeKubeCluster
## Step by Step guide to setup your Multinode Cluster 


> I have configured my setup as 1 Master Server and 2 Worker Node - where my PODs will be launched by Master. Install Docker-ce , kubelet on your Master. 
> Also install kubeadm to manage(bootstrap) the cluster. You can follow below links to install docker, kubelet,kubectl,kubeadm, disable firewall(not recommended for prod),      * SELinux disablement

> https://download.docker.com/linux/centos/7/x86_64/stable/

> https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

![snap1](https://user-images.githubusercontent.com/51450944/87507779-fb514980-c63b-11ea-85b9-f238374617ce.PNG)

![snap2](https://user-images.githubusercontent.com/51450944/87507780-fb514980-c63b-11ea-8ef9-7cabada50bf0.PNG)




Kubernetes in RHEL ⅞ version doesn’t support cgroup drive so we need to change the cgroup driver for docker to systemd - https://github.com/kubernetes/minikube/issues/4770

**etc/docker/daemon.json**

Enter below line

![snap3](https://user-images.githubusercontent.com/51450944/87507782-fb514980-c63b-11ea-9659-d5bdb4ffd060.PNG)

**Disable swap** 
vim /etc/fstab

![snap4](https://user-images.githubusercontent.com/51450944/87507783-fbe9e000-c63b-11ea-8a3d-6260693367a0.PNG)

**Install - iproute-tc, also iptable=1**

![snap5](https://user-images.githubusercontent.com/51450944/87507784-fbe9e000-c63b-11ea-9fe2-5771d9141eb5.PNG)


Now this master VM is installed and configured we can now clone this and create 2 node
Set your hostname(hostnamectl set-hostname Master/Slave1/Slave2) before working inside cluster so they can connect and authenticate using hostname

> Initialize your Master VM to make it master and provide a range of IP to provide to pods. 


> This will start generating all the relevant services for master as you can see below

![snap6](https://user-images.githubusercontent.com/51450944/87507785-fbe9e000-c63b-11ea-9348-d76d8374ec5b.PNG)


> Now in order to start using cluster, either master or worker you need to run below kubeconfig command as directed on screen

![snap7](https://user-images.githubusercontent.com/51450944/87507786-fbe9e000-c63b-11ea-878a-78cc8c7e65cb.PNG)


> By default 4 NameSpace will be created on your master. Also you can check services running inside the namespace - kube-system

![snap8](https://user-images.githubusercontent.com/51450944/87507788-fbe9e000-c63b-11ea-8948-c9925b06d1f4.PNG)

> In order to configure networking between the pod i am going to run CNI plugin flannel. You can check more about this on the link.          https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.ym


> As soon as you create a flannel network. Coredns container will reach in running state & our MA\aster is ready. Next you can use Kubeadm join command on your slave node as     
  describe in above screenshot.

![snap10](https://user-images.githubusercontent.com/51450944/87507790-fc827680-c63b-11ea-97b3-62fb396e052b.PNG)

Now use kubectl get nodes and you can see your MultiNode Cluster is ready :)

![snap11](https://user-images.githubusercontent.com/51450944/87507791-fc827680-c63b-11ea-96d5-ad74b8ed63e0.PNG)
