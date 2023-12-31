# Display out via usb-camera
The idea is to create a usb gadget and show the desktop throught there
- Network gadget, share stream via local http
- UVC camera, share stream via camera
- adb protocol and scrcpy?

### Dummy Screen
```sh
# https://askubuntu.com/questions/453109/add-fake-display-when-no-monitor-is-plugged-in
# https://forums.raspberrypi.com/viewtopic.php?t=294588
# https://techoverflow.net/2019/02/23/how-to-run-x-server-using-xserver-xorg-video-dummy-driver-on-ubuntu/
# https://tldp.org/HOWTO/Framebuffer-HOWTO/x1232.html
```

### Capture Desktop to dummy camera
```sh
# https://github.com/umlaeute/v4l2loopback
# https://superuser.com/questions/411897/using-desktop-as-fake-webcam-on-linux
sudo apt-get install raspberrypi-kernel-headers v4l2loopback-dkms
sudo modprobe v4l2loopback video_nr=100 exclusive_caps=1
ffmpeg -f x11grab -i :0.0+0,0 -vcodec mjpeg -pix_fmt yuv420p -f v4l2 /dev/video100
uvc-gadget -f /dev/fb0 -u /dev/video0
# set vnc resolution to 1920x1080
# https://superuser.com/questions/1585515/how-do-i-stream-jpgs-to-v4l2loopback-with-ffmpeg
```

For wayland:
```sh
# https://github.com/ammen99/wf-recorder/wiki
```

### UVC Gadget for Raspberry PI
```sh
# https://gist.github.com/justinschuldt/36469e2a89d95ef158a8c4df091e9cb4
# https://github.com/geerlingguy/pi-webcam
modprobe usb_f_fs
# https://www.raspberrypi.com/tutorials/plug-and-play-raspberry-pi-usb-webcam/
sudo uvc-gadget -u /dev/video0 -v /dev/video100 -r 30
# https://github.com/peterbay/uvc-gadget
```

### Remote controlling
```sh
# https://github.com/dottedmag/x2x
# https://superuser.com/questions/322658/control-linux-console-session-keyboard-over-ssh
```

---

## Configuring from Scratch (RPIOS)
```bash
echo "dtoverlay=dwc2,dr_mode=otg" | sudo tee -a /boot/firmware/config.txt
echo " modules-load=dwc2,libcomposite" | sudo tee -a /boot/cmdline.txt
git clone https://github.com/peterbay/uvc-gadget
cd uvc-gadget && make
sudo cp ./uvc-gadget /usr/bin/
sudo cp ./gadget-framebuffer.sh /usr/bin/uvcgfb
sudo chmod +x /usr/bin/uvcgfb
sudo cp uvcgfb.service /etc/systemd/system/
sudo systemctl enable uvcgfb.service
sudo apt install xserver-xorg-video-dummy

# Start X on dummy display
sudo startx -- -config dummy-1920x1080.conf
# Manually for now!
sudo uvc-gadget -f /dev/fb0 -u /dev/video0
```
