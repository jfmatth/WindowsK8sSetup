# WindowsK8sSetup
Working with Kubernetes on Windows is a less than excellent experience.  Docker makes a great free product, and Minikube is amanzing, but most examples on the Internet involve some form of BASH scripting, etc.

So, I'm documenting for everyone, my Kubernetes setup on Windows.

## What pieces are there?
- A local Hypervisor - Hyper-V or [VirtualBox](https://virtualbox.org).  I prefer Hyper-V and I'll explain later
- [Multipass](https://mutipass.run) from Ubuntu
- An SSH Keypair if you have one, if not, we'll create one for you
- A Kubernetes distribution ([MicroK8s](https://microk8s.io) or [K3s](https://k3s.io)).  I will use K3s in this repo
- [VSCode](https://code.visualstudio.com/)

## Why so many pieces?
1. The Hypervisor allows me to run a full Linux distro. 
2. Multipass manages the VM's under that Hypervisor, you'll see why that's important
3. Kubernetes - dahh
4. SSH - Allows us to use VSCode on the VM
5. VSCode - The best editor for Windows

## Let' get started

### Install a Hypervisor
I won't document the Hyper-V or Virtualbox installs, they are pretty straightforward.  Just make sure you can build VM's manually with them before proceeding.

### Install Multipass
This is a very cool (free) product from Unbuntu.  It quickly allows you to spin up, manage, shell, exec or delete Ubuntu VM's on your Windows machine without learning anything.  There are some very cool shortcuts built in, and a great GUI interface as well.

Installing it is just a matter of following the instructions on their website.  Once it's installed, run a quick test.
```
multipass launch --name primary --cpus 2 --mem 2g
```
I use Multipass so much, I've created another repo for all my Multipass goodness, called (you guessed it) [Multipass goodness](https://github.com/jfmatth/multipass-goodness.git).  I'll be using two of my scripts (buildvm.cmd and mp.cmd) throughout my docs.

Multipass has a feature for it's 'primary' VM that can be accessed in one of two ways: 1. Click the taskbar app and click "Open Shell" or 2. Ctrl-Alt-U.  Try clicking Ctrl-Alt-U, if you've not build a primary VM, it will begin creating one for you and put you into the shell.  From now on, unless you power it down, Ctrl-Alt-U will get you back to the shell, and you can launch it as many times as you want.  I use my Primary VM as a staple of everything linux on my machine.  I typically don't delete it like other VM's.

Another feature of Multipass which is handy, it mounts your Windows home directory as ~/Home on your VM, via an SSHFS file system.  Check ~/Home and you'll see what I mean.  It provides a quick and easy way to move / access files between the VM and Windows.

A note about Hyper-V in case you didn't realize it.  Hyper-V provides a 'Default Switch' which is a Nat'd interface for VM's to the outside world, but it also puts your Windows network on the same network.  What this means is you have your VM's network locally available to Windows program w/o any work.  This isn't something I've found on other Hypervisors, and definately not on Virtualbox.  I think it's worth the 50 bucks or so you pay to upgrade to Windows Pro or better.

### SSH keypair
If you have a keypair, make sure it works from Windows.  Typically if you install the GIT version for Windows, in includes an SSH client.  Once you have a keypair, I load my public part (id_rsa.pub) onto github for installing onto VM's I build (my buildvm.cmd script does that too).

### Kubernetes(k3s) install
So you could install Kubernetes onto the primary VM that Multipass creates, but as I mentioned, I keep the primary VM pretty clean for my personal usage.

I would create a new VM called kubernetes from the command line with my buildvm.cmd script
```
buildvm kubernetes nano
```
**if you use buildvm.cmd, make sure to adjust the script for your GitHub key pairs**

In a bit you should have a multipass, Ubuntu based VM called kubernetes.

You can get into that VM in one of three ways:
- SSH from the primary VM (your keys need to be working from the primary VM first)
- SSH from Windows
- Use the Mutipass GUI link to Open Shell into that VM

Once in the VM, lets install K3s
```
curl -sfL https://get.k3s.io | sh -
```





