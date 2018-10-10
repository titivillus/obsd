Instal.lacio des de Debian Live...

```
sudo apt-get install qemu ovmf
cp /usr/share/qemu/OVMF.fd bios.bin
sudo qemu-system-x86_64 -L . -curses -drive file=miniroot63.fs -drive file=/dev/sda -net nic -net user
```
```
-cpu host
```
```
sudo apt-get install qemu ovmf
cp /usr/share/qemu/OVMF.fd bios.bin

sudo qemu-system-x86_64 -L . -boot d -curses -smp 4 -drive format=raw,file=install63.iso,media=cdrom -drive file=/dev/sda -net nic -net user

alt+2 quit .... per sortir

sudo qemu-system-x86_64 -L . -curses -smp 4 -drive file=/dev/sda -net nic -net user
```
En comptes de `-curses` -> `-nographic` i per sortir Ctr-A després X
