# Display out via usb-camera
The idea is to create a usb gadget and show the desktop throught there
- Network gadget, share stream via local http
- UVC camera, share stream via camera
- adb protocol and scrcpy?

### Dummy Screen
```sh
# https://askubuntu.com/questions/453109/add-fake-display-when-no-monitor-is-plugged-in
```

### Capture Desktop to dummy camera
```sh
# https://github.com/umlaeute/v4l2loopback
# https://superuser.com/questions/411897/using-desktop-as-fake-webcam-on-linux
sudo apt-get install raspberrypi-kernel-headers v4l2loopback-dkms
modprobe v4l2loopback video_nr=100
ffmpeg -probesize 100M -framerate 30 -f x11grab -video_size 1280x720 -i :0.0+0,0 -vcodec rawvideo -pix_fmt yuv420p -f v4l2 /dev/video100
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
```

### Remote controlling
```sh
# https://github.com/dottedmag/x2x
# https://superuser.com/questions/322658/control-linux-console-session-keyboard-over-ssh
```
