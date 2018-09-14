Aquest és un repositori particular amb algunes anotacions per a la instal·lació de l'OpenBSD. No està complet i no serveix per copiar i enganxar.


```
# echo 'https://ftp.hostserver.de/pub/OpenBSD/' > /etc/installurl
# syspatch
# shutdown -r now
```

```
# echo 'permit nopass :wheel as root' > /etc/doas.conf
# pkg_add nano firefox-i18n-ca
[...]
# ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
# ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
# ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
# ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
# rcctl enable messagebus
# rcctl start messagebus
```

Probarem amb "2"

`# echo 'machdep.allowaperture=2' > /etc/sysctl.conf`


https://www.romanzolotarev.com/openbsd/install.html :

```
# cp /etc/fstab /etc/fstab.bak`
# sed -i 's/rw/rw,softdep,noatime/' /etc/fstab
```

Canvio els límits de l'Staff

```
# cp /etc/login.conf /etc/login.conf.bak
# sed -i 's/datasize-cur=1536M/datasize-cur=8192M/' /etc/login.conf
# cap_mkdb /etc/login.conf
```

Aquests, de moment, no:

```
# sed -i 's/datasize-max=768M/datasize-max=8192M/' /etc/login.conf
# rcctl set ntpd flags -s
```

Seguim...

```
# echo up > /etc/hostname.em0 
   
# cat << 'EOF' > /etc/hostname.iwm0
nwid Papageno
wpakey XusHECafmFDCxYQNpNEd
up
EOF

```

```
# cat << 'EOF' > /etc/hostname.trunk0
trunkproto failover trunkport em0
trunkport iwm0
dhcp
EOF
```

```
# mkdir -p /etc/X11/xorg.conf.d
# cat << 'EOF' > /etc/X11/xorg.conf.d/20-intel.conf
Section "Device"
        Identifier "Intel Graphics"
        Driver "intel"
EndSection
EOF
```

Instal·laré alguna cosa per anar fent...

`# pkg_add openbox rxvt-unicode inconsolata-font mozilla-dicts-ca thunar thunar-archive thunar-media-tags geany`

Configuració d' rc.conf.local:

`# rcctl enable multicast messagebus avahi_daemon`

### Com a usuari...

Al directori d'usuari:

`$ echo 'export LC_CTYPE="ca_ES.UTF-8"' >> .profile`

Instal·lació dels diccionaris:

```
$ cd
$ mkdir dict
$ cd dict
$ cp /usr/local/share/mozilla-dicts/ca.* .
$ mv ca.aff ca_ES.aff                                                  
$ mv ca.dic ca_ES.dic
$ cd

```

```
$ cat << 'EOF' >> .Xdefaults
! ------------------------------------------------------------------------------
! URxvt configs
! ------------------------------------------------------------------------------

URxvt.geometry: 	80x24
URxvt*letterSpace: 	0
URxvt.lineSpace: 	0
URxvt*internalBorder:   24
URxvt*externalBorder:   0
URxvt*depth:            32
URxvt*saveline:         2000
URxvt*termName:         rxvt-256color
URxvt*iso14755:         false
URxvt*scrollBar:        false
URxvt*scrollBar_right:  false
URxvt.copyCommand:      xclip -i -selection clipboard
URxvt.pasteCommand:     xclip -o -selection clipboard
URxvt.keysym.Shift-Up:	command:\033]720;1\007
URxvt.keysym.Shift-Down:command:\033]721;1\007
URxvt.urlLauncher:      firefox
URxvt.underlineURLs:    true
URxvt.urlButton:        1
URxvt*buffered:         false          
URxvt.urgentOnBell: True
URxvt.xftAntialias:     true
URxvt.font:             xft:inconsolata:pixelsize=16:antialias=true:hinting=true 
URxvt.boldFont:         xft:inconsolata:pixelsize=16:antialias=true:hinting=true
URxvt.italicFont:       xft:inconsolata:pixelsize=16:antialias=true:hinting=true
URxvt.bolditalicFont: 	xft:inconsolata:pixelsize=16:antialias=true:hinting=true
EOF
```

```
$ cat << 'EOF' > .xinitrc
#!/bin/sh
# $OpenBSD: xinitrc.cpp,v 1.13 2015/10/17 08:25:11 matthieu Exp $
userresources=$HOME/.Xresources
usermodmap=$HOME/.Xmodmap
sysresources=/etc/X11/xinit/.Xresources
sysmodmap=/etc/X11/xinit/.Xmodmap
# merge in defaults and keymaps
if [ -f $sysresources ]; then
    xrdb -merge $sysresources
fi
if [ -f $sysmodmap ]; then
    xmodmap $sysmodmap
fi
if [ -f "$userresources" ]; then
    xrdb -merge "$userresources"
fi
if [ -f "$usermodmap" ]; then
    xmodmap "$usermodmap"
fi
# if we have private ssh key(s), start ssh-agent and add the key(s)
id1=$HOME/.ssh/identity
id2=$HOME/.ssh/id_dsa
id3=$HOME/.ssh/id_rsa
id4=$HOME/.ssh/id_ecdsa
id5=$HOME/.ssh/id_ed25519
if [ -z "$SSH_AGENT_PID" ];
then
	if [ -x /usr/bin/ssh-agent ] && [ -f $id1 -o -f $id2 -o -f $id3 -o -f $id4 -o -f $id5 ];
	then
		eval `ssh-agent -s`
		ssh-add < /dev/null
	fi
fi

export LC_CTYPE=ca_ES.UTF-8
export LC_MESSAGES=ca_ES.UTF-8
export MOZ_ACCELERATED=1

# Per a que carregui tots els tipus
xset fp default 
for font in /usr/local/share/fonts/* ; do 
        xset fp+ $font 
done 
xset fp rehash
 
xsetroot -solid black &
urxvt &
openbox-session

# start some nice programs
#xclock -geometry 50x50-1+1 &
#xconsole -iconic &
#xterm -geometry 80x24 &
#fvwm || xterm


if [ "$SSH_AGENT_PID" ]; then
	ssh-add -D < /dev/null
	eval `ssh-agent -s -k`
fi
EOF
```
