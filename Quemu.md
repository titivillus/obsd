Instal.lacio des de Debian Live...

```
sudo apt-get install qemu ovmf
cp /usr/share/qemu/OVMF.fd bios.bin
sudo qemu-system-x86_64 -L . -curses -drive file=miniroot63.fs -drive file=/dev/sda -net nic -net user
```
```
-cpu host
```
````
qemu-system-x86_64 -L .	-boot d \
	 		-k es \
			-cpu host \
			-m 4G \
 			-curses \
 			-cdrom file=install63.iso \
 			-drive file=/dev/sda \
 			-net nic \
			-net user
      ```
En comptes de `-curses` -> `-nographic` i per sortir Ctr-A després X
