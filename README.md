# sia-rpi4

This is a first version and will be (hopefuly) be udpated.

Install and boot an official raspberian image (32bit) and follow the instructions of [sakaki-/bcm2711-kernel-bis](https://github.com/sakaki-/bcm2711-kernel-bis#example-2-updating-an-existing-booted-image). You only have to run the wget and sync command and add some lines to the config.txt. This will install an 64 bit kernel on the 32 bit userspace.

install golang
```
sudo apt-get install golang
```


set GOARCH to arm64 (cross compile since userspace is 32bit)
```
echo "export GOARCH=arm64
export GOOS=linux" >> ~/.bashrc
bash #to start new bash with the new variables
```

self compile siacoin binaries (siac siad via https://gitlab.com/NebulousLabs/Sia) with
```
git clone https://gitlab.com/NebulousLabs/Sia siasrc
cd siasrc && make
```
The new binaries can be found under "~/go/bin/linux_arm64/", but we just add them to the path as follows.

Add siad siac to PATH so we can run it anywhere in the console
```
echo 'PATH=~/go/bin/linux_arm64:$PATH' >> ~/.profile
sudo reboot
```

### Update siac siad binaries
If there is a new version or you want to pull the latest changes of sia run
```
cd siasrc && git pull && make
```
With this command the new changes will be pulled from gitlab and compiled into the "~/go/bin/linux_arm64/" folder which is already included in the path and the new binaries should be used automatically, just restard siad.

### Set bandwidth limits
We use trickle to limit the throughput only for the process
```
sudo apt-get install trickle
```
we let trickle start siad to do its limiting, units are in KB/s
```
mkdir sia
cd sia #first change the directory to the folder where sia will store its data
trickle -s -d 50000 -u 30000 siad -M gctwr
```
This will launch siad without the host and using only the renter.


