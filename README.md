Refer below for practicing CKA "Certified Kubernetes Administrator" for kubernetes Upgrade Cluster.


kubernetes Upgrade cluster steps:
==============================

[1] Check the node version :
# kubectl get nodes 
NAME           STATUS   ROLES    AGE     VERSION
controlplane   Ready    master   9m30s   v1.19.0
node01         Ready    <none>   8m47s   v1.19.0
root@controlplane:~# 

[2] Check the current version and version that cluster need to be upgrade to. 

[3] To check all the version of cluster components. 
#  kubeadm version;kubectl version; kubectl --version


[4] First start with upgrade of master node, ensure that there are no application/deployment running on master.

--------------
--[5] Step to upgrade the Master/controlplane node :
--------------
On Master Node: 
[5.a] # sudo apt update
[5.b] # apt install kubeadm=1.20.0-00

[5.c] # kubeadm version 
--It should be 1.20.0 now

[5.d] # kubeadm upgrade plan
You can now apply the upgrade by executing the following command:
        kubeadm upgrade apply v1.20.15

# kubeadm upgrade apply v1.20.0  [[This will work perfectly fine, minde the Major[1]/Minor[20]/Patch version[0] = 1.20.0]  
upgrade/versions] Cluster version: v1.19.0
[upgrade/versions] kubeadm version: v1.20.0
[upgrade/confirm] Are you sure you want to proceed with the upgrade? [y/N]

[5.e] # kubectl get node 
--It will still show the old version of the master node 

[5.f] # apt install kubectl=1.20.0-00 
[5.g] # apt install kubelet=1.20.0-00

[5.h] # systemctl restart kubelet
[5.i] # kubectl get node  [ It will still show the old 1.19.0 version]
[5.j] # sudo systemctl daemon-reload
      # sudo systemctl restart kubelet 
[5.k] # kubectl get node  [ It will still show the lastest version now]
--It will show the latest version of 1.20.0

--------------
[6] Step to upgrade the Node01: 
--------------
--# ssh node01
# sudo apt update
# apt install kubeadm=1.20.0-00
# sudo kubeadm upgrade node

-- # ssh controlplane and drian the node01 from the controlplane
# sudo kubectl drain node01--ignore-daemonsets  [Execute this command from the controlplane]

--# ssh node01 and install the kubectl and kubelet
# apt install kubectl=1.20.0-00 kubelet=1.20.0-00 [correct command,  don't specify comma or colon in between with apt install of multiple components]
 
# sudo systemctl daemon-reload
# sudo systemctl restart kubelet 


-- Ensure that controlplane and node01 has correct version.
===========================================================
