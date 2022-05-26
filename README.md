# Setting up ntopng community (nDPI, nprobe) on vyos 1.4 rolling

I have this on here for my personal reference since it took me a while to figure it out. I also hope it might help someone on how to set up ntopng on vyos without wasting a ton of time like I did. I have tested this on the vyos1.4 rolling image and while I will not be documenting a vyos image installation or setup, there are a few basic stuff that needs to be set up on the vyos router before ntopng can be installed.

On the vyos router, log in with admin privileges and open a root user session:
```
sudo su
```
Update the sources.list for the particular version of debian the vyos image is based on. In my case with vyos 1.4 rolling, I see it is based on debian bullseye so I add the bullseye repo.
```
nano /etc/apt/sources.list
```
Add the following lines to the sources.list
```
deb http://deb.debian.org/debian bullseye main contrib non-free
deb http://deb.debian.org/debian bullseye-updates main contrib non-free
deb http://deb.debian.org/debian bullseye-backports main contrib non-free
deb http://security.debian.org/debian-security/ bullseye-security main contrib non-free
```
Run a quick update (no upgrade/install at this time)
```
apt update
```
Download the ntop repo installer from the packages.ntop.org site for the particular version of debian the vyos is based on and install it. This will add the repo for ntopng and associated tools and gpg keys. You have a choice of binaries for the nightly builds or the stable version. More info can be found at https://packages.ntop.org/
```
wget https://packages.ntop.org/apt-stable/bullseye/all/apt-ntop-stable.deb
apt install ./apt-ntop-stable.deb
```
You should see something like the following added to a newly created **/etc/apt/sources.list.d/ntop-stable.list**
```
deb https://packages.ntop.org/apt-stable/1.4-rolling-202205190217/ x64/
deb https://packages.ntop.org/apt-stable/1.4-rolling-202205190217/ all/
```
The installer probably picks up the version from the **/etc/os-release** and since this is vyos but since we need debian sources and repo, replace the version in the URI's with whatever your current vyos is based on. In my case this is **bullseye**.
```
deb https://packages.ntop.org/apt-stable/bullseye/ x64/
deb https://packages.ntop.org/apt-stable/bullseye/ all/
```
Run an update
```
apt update
```
Now you should good to follow the instructions on the ntop installation page for debian based systems as documented at https://packages.ntop.org/apt-stable/
```
apt install pfring-dkms nprobe ntopng n2disk cento
```
You could optionally install the ZC drivers
```
apt install pfring-drivers-zc-dkms
```
Just in case you're wondering if I did an **apt upgrade** - no, I did not do that in case it breaks something in my vyos installation and I am not ready to spend time on that right now.

Set the ntopng service to autostart if not already
```
systemctl enable ntopng
systemctl start ntopng
```
I had a successful installation with the exact steps above and have ntopng running without any issues thus far. While I can hardly call myself an expert on vyos nor ntopng, I believe I am pretty much set to start learning now.

Big thanks to the folks at [vyos](https://vyos.io/) and [ntop](https://www.ntop.org/) for making such amazing software available for everyone and for free!
