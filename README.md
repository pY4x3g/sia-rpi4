# sia-rpi4

This doc is currently under work, here are only some notes:

official raspberian image
Kernel from https://github.com/sakaki-/bcm2711-kernel-bis

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

add siad siac to PATH so we can run it anywhere in the console
```
echo 'PATH=~/go/bin/linux_arm64:$PATH' >> ~/.profile
sudo reboot
```

### Update siac siad binaries
```
cd siasrc && git pull && make
```
with this command the new changes will be pulled from gitlab and compiled into the "~/go/bin/linux_arm64/" folder which is already included in the path and the new binaries should be used automatically, just restard siad.

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


