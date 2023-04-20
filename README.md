# Vagrant-Virtualbox-ubuntu20.04
Create the VM using Vagrant + Virtual Box for Ubuntu 20.04

How do change the Internal IP of Kubernets nodes

Before Changes. We could see the below Internal IP especially for Vagrant and Virtual Box case

ubuntu@kubemaster:~$ kubectl get nodes -o wide
NAME         STATUS   ROLES           AGE    VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
kubemaster   Ready    control-plane   116m   v1.26.0   10.0.2.15   <none>        Ubuntu 20.04.5 LTS   5.4.0-147-generic   containerd://1.6.20
kubeslave1   Ready    <none>          103m   v1.26.0   10.0.2.15   <none>        Ubuntu 20.04.5 LTS   5.4.0-139-generic   containerd://1.6.20
kubeslave2   Ready    <none>          103m   v1.26.0   10.0.2.15   <none>        Ubuntu 20.04.6 LTS   5.4.0-139-generic   containerd://1.6.20
ubuntu@kubemaster:~$ 


vi /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

Add below entry at the last line

ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS  \
  --node-ip=192.168.0.92
  
Save and quit


sudo systemctl daemon-reload

sudo systemctl restart kubelet.service

Then check the status of Internal IP

ubuntu@kubemaster:~$ kubectl get nodes -o wide
NAME         STATUS   ROLES           AGE    VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
kubemaster   Ready    control-plane   116m   v1.26.0   192.168.0.90   <none>        Ubuntu 20.04.5 LTS   5.4.0-147-generic   containerd://1.6.20
kubeslave1   Ready    <none>          103m   v1.26.0   192.168.0.91   <none>        Ubuntu 20.04.5 LTS   5.4.0-139-generic   containerd://1.6.20
kubeslave2   Ready    <none>          103m   v1.26.0   192.168.0.92   <none>        Ubuntu 20.04.6 LTS   5.4.0-139-generic   containerd://1.6.20
ubuntu@kubemaster:~$ 

