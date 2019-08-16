# sia-rpi4

This doc is currently under work, here are only some notes:

official raspberian image
Kernel from https://github.com/sakaki-/bcm2711-kernel-bis
install go

set GOARCH to arm64 (cross compile since userspace is 32bit)

self compile: https://gitlab.com/NebulousLabs/Sia 
via
```
go get -u gitlab.com/NebulousLabs/Sia/...
```
or
```
git clone https://gitlab.com/NebulousLabs/Sia
cd Sia && make
```

add siad siac to PATH so we can run it anywhere in the console
```
echo 'PATH=~/go/bin/linux_arm64:$PATH' >> ~/.profile
sudo reboot
```
