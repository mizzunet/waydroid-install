# How to use android 11 images in waydroid

## Make pre-installed waydroid images folder
```
sudo mkdir -p /usr/share/waydroid-extra/images
```

## Download android 11 images (choose one) on to Downloads folder
```
cd ~/Downloads
```
- https://mega.nz/folder/N10jGA4a#j8tF2-6LY6Qq5Da2zV0Z7g
```
  sudo 7z e -o/usr/share/waydroid-extra/images system.img.7z
  sudo 7z e -o/usr/share/waydroid-extra/images vendor.img.7z
```
  or

- https://sourceforge.net/projects/blissos-dev/files/waydroid/lineage/lineage-18.1/
```
  sudo unzip -d /usr/share/waydroid-extra/images '*waydroid*.zip'
```

## Initialize use pre-installed waydroid images
```
sudo waydroid init -f -i /usr/share/waydroid-extra/images
```

## Enable kernel unprivileged bpf
```
sudo sysctl -w kernel.unprivileged_bpf_disabled=0
```

## Enable memfd
```
echo "sys.use_memfd=true" | sudo tee -a /var/lib/waydroid/waydroid_base.prop
```

## Enable memfd in waydroid images
```
mkdir /tmp/rootfs
sudo mount -o rw /usr/share/waydroid-extra/images/system.img /tmp/rootfs
sudo sed -i '/setprop sys.use_memfd/s/false/true/' /tmp/rootfs/system/etc/init/hw/init.rc
sudo umount /tmp/rootfs && rmdir /tmp/rootfs 
```

## Change ApiLevel = 30
- Debian or Ubuntu
```
  sudo sed -i '/ApiLevel/s/29/30/' /etc/gbinder.d/anbox.conf
```

- Arch Linux
```
  sudo sed -i '/ApiLevel/s/29/30/' /etc/gbinder.conf
```

- Fedora
```
  sudo sed -i '/ApiLevel/s/29/30/' /etc/gbinder.d/waydroid.conf
```

## Run waydroid container
```
sudo systemctl stop waydroid-container.service
sudo killall waydroid
sudo systemctl start waydroid-container.service || sudo waydroid start container
```

## Run waydroid session, Open new tab (Ctrl+Alt+T)
```
waydroid session start
```

please wait until you see message "Android with user 0 is ready"

## Run waydroid ui after it ready, Open new tab again
```
waydroid show-full-ui
```


Good luck. :)


last updated at 2022-06-27

