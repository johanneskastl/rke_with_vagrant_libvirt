# rke_with_vagrant_libvirt

Vagrant setup for building a RKE cluster with three controlplane nodes using the vagrant libvirt provider 

This setup creates three controlplane VMs using rke.

Default OS is openSUSE Leap 15.2, but that can be changed in the Vagrantfile. Same holds true for the sizing of the machines.

Please note that changing the OS in the Vagrantfile requires to rewrite the shell provisioner that make sure all prerequisites for rke are met (docker installed and running, ...).

**There is a branch called `cluster_with_workers` which enhances this setup to install an additional worker-only node.**

## Vagrant

1. You need vagrant obviously. And libvirt. And rke.
2. Fetch the box, per default this is `opensuse/Leap-15.2.x86_64`, using `vagrant box add opensuse/Leap-15.2.x86_64`.
3. Make sure the machine where you want to run this on has enough RAM to allow running the three VMs. If not, adjust the sizes in the `Vagrantfile`.
4. Run `vagrant up`
5. Run `kubectl --kubeconfig kube_config_cluster.yml get nodes` and you should see your three controlplane nodes.
6. Party!

## Adjusting size in the Vagrantfile

In case you want or need to change the VM sizing, change the following lines in the `Vagrantfile`:
```
        lv.memory = 2048
        lv.cpus = 2
```

## Creating your own ssh keys

This setup uses a ssh private key that is delivered with this repository. This is a NO-GO for productive use cases, and this key should be considered insecure and for testing purposes only.

In case you want to create your own key, run the following commands:

```bash
rm ssh_key_rke ssh_key_rke.pub
ssh-keygen -t ecdsa -f ssh_key_rke -C "rke_with_vagrant_libvirt" -P ""
```

Please make sure you do not accidentally commit your new ssh key files somehow (they are not excluded via `.gitignore`)...

## Cleaning up

You can remove your vagrant VMs using `vagrant destroy`, but you need to manually remove the following files before starting again:
```bash
cluster.rkestate
kube_config_cluster.yml
```

Otherwise rke will try to restore the state saved in the `cluster.rkestate` file. And will fail horribly...
