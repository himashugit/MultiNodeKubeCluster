# MultiNodeKubeCluster
## Step by Step guide to setup your Multinode Cluster 


> I have configured my setup as 1 Master Server and 2 Worker Node - where my PODs will be launched by Master. Install Docker-ce , kubelet on your Master. 
> Also install kubeadm to manage(bootstrap) the cluster. You can follow below links to install docker, kubelet,kubectl,kubeadm, disable firewall(not recommended for prod),      * SELinux disablement

https://download.docker.com/linux/centos/7/x86_64/stable/

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/






Kubernetes in RHEL ⅞ version doesn’t support cgroup drive so we need to change the cgroup driver for docker to systemd - https://github.com/kubernetes/minikube/issues/4770

etc/docker/daemon.json

Enter below line


Disable swap 
vim /etc/fstab


Install - iproute-tc, also iptable=1


Now this master VM is installed and configured we can now clone this and create 2 node
Set your hostname(hostnamectl set-hostname Master/Slave1/Slave2) before working inside cluster so they can connect and authenticate using hostname

Now Initialize your Master VM to make it master and provide a range of IP to provide to pods



This will start generating all the relevant services for master as you can see below


Now in order to start using cluster, either master or worker you need to run below kubeconfig command as directed on screen

By default 4 NameSpace will be created on your master. Also you can check services running inside the namespace - kube-system



In order to configure networking between the pod i am going to run CNI plugin flannel. You can check more about this on the link. https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.ym



lAs soon as you create a flannel network. Coredns container will reach in running state & our MA\aster is ready. Next you can use Kubeadm join command on your slave node as describe in above screenshot.





Now use kubectl get nodes and you can see your MultiNode Cluster is ready :)

