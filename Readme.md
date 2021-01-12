## Instructions

1) Get Gpu settings

```Shell
lspci | grep VGA
```

for AMD it should look something like

```Shell
00:02.0 VGA compatible controller: Intel Corporation UHD Graphics (rev 02)
0a:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Fiji [Radeon R9 FURY / NANO Series] (rev cb)

```

for NVIDIA it should look something like

```Shell
00:02.0 VGA compatible controller: Intel Corporation Iris Pro Graphics 580 (rev 09)
08:00.0 VGA compatible controller: NVIDIA Corporation GP104 [GeForce GTX 1080] (rev a1)

```

2) In my case, my bus id is “08:00.0”. You will need this bus id for a custom Xorg config.

AMD
(AMD Instructions Arch Wiki)[https://wiki.archlinux.org/index.php/AMDGPU]

```
sudo vim /etc/X11/xorg.conf.d/20-amdgpu.conf
```

```Shell

Section "Device"
     Identifier "AMD"
     Driver "amdgpu"
     Option "NoLogo" "true"
     Option "AllowExternalGpus" "true"
     BUSID "PCI:a:0.0"           
EndSection
```

Nvidia

(Nvidia Instructions Arch Wiki)[https://wiki.archlinux.org/index.php/NVIDIA]

```Shell
sudo vim /etc/X11/xorg.conf.d/20-nvidia.conf
```

```Shell
Section "Device"
        Identifier "Nvidia Card"
        Driver "nvidia"
        VendorName "NVIDIA Corporation"
        Option "NoLogo" "true"
        Option "AllowExternalGpus" "true"
        BusID "PCI:8:0:0"
EndSection
```

3) Blacklist the Intel video driver to avoid any potential conflicts. 
   * I put the following into my 
   
```Shell
sudo vim /etc/modprobe.d/blacklist.conf
```

```Shell
blacklist i915
install i915 /bin/false
```