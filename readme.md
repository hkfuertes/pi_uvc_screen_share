# Display out via usb-camera
The idea is to create a usb gadget and show the desktop throught there
- Network gadget, share stream via local http
- UVC camera, share stream via camera
- adb protocol and scrcpy?

### Caputure Desktop to dummy camera
```sh
# https://superuser.com/questions/411897/using-desktop-as-fake-webcam-on-linux
modprobe v4l2loopback
ffmpeg -probesize 100M -framerate 30 -f x11grab -video_size 1920x1080 -i :0.0+0,0 -vcodec rawvideo -pix_fmt yuv420p -f v4l2 /dev/videoN
```

### UVC Gadget for Raspberry PI
```sh
# https://gist.github.com/justinschuldt/36469e2a89d95ef158a8c4df091e9cb4
# https://github.com/geerlingguy/pi-webcam
```
