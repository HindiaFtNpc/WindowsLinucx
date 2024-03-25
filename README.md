[![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/colored.png)](#table-of-contents)
<p align="center">
    <img src="https://raw.githubusercontent.com/NeeasTooID/Static-HTML/main/media/img0_3840x2160.jpg" width="100%" style="margin-left: auto;margin-right: auto;display: block;">
</p>

> Run Sh as usual and don't forget to change the config.json!
> 
> With this you can run Windows on Linux, Ubuntu, Debian and so on
> 
> Exemple iso "https://drive.massgravel.workers.dev/en-us_windows_10_iot_enterprise_ltsc_2021_x64_dvd_257ad90f.iso" Windows Ltsc 2021
>
> Don't use this script for anything illegal! 

## Setting up a Windows VM using QEMU

### Prerequisites
- Ensure you have `qemu-kvm` installed.
- Have sufficient disk space (at least 60GB) for the VM.

### Run the following commands ( Use Codespace ) :

```bash
# Remove files in /tmp and clear screen
cd /tmp && sudo rm -r * && clear

# Download Windows 10 ISO
wget -O win.iso "https://drive.massgravel.workers.dev/en-us_windows_10_iot_enterprise_ltsc_2021_x64_dvd_257ad90f.iso"

# Download BIOS
wget https://github.com/HindiaFtNpc/WindowsLinucx/raw/main/bios64.bin

# Update apt repositories
sudo apt update

# Install qemu-kvm
sudo apt install qemu-kvm -y

# Create virtual hard disk (60GB)
qemu-img create -f raw win.img 60G

# Run QEMU
sudo qemu-system-x86_64 -m 4G -smp 2 -cpu host -boot order=c \
-drive file=win.iso,media=cdrom -drive file=win.img,format=raw \
-device usb-ehci,id=usb,bus=pci.0,addr=0x4 -device usb-tablet \
-vnc :0 -smp cores=2 -device e1000,netdev=n0 -netdev user,id=n0 \
-vga qxl -accel kvm -bios bios64.bin

# Install Playit.gg ( Opsional )
curl -SsL https://playit-cloud.github.io/ppa/key.gpg | gpg --dearmor | \
sudo tee /etc/apt/trusted.gpg.d/playit.gpg >/dev/null
echo "deb [signed-by=/etc/apt/trusted.gpg.d/playit.gpg] \
https://playit-cloud.github.io/ppa/data ./" | sudo tee \
/etc/apt/sources.list.d/playit-cloud.list
sudo apt update
sudo apt install playit

