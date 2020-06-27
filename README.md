# ansible-motion

## Usages

### example `playbook.yml`

```yaml
---
- hosts: all
  become: true

  roles:
    - role: ansible-motion
      motion_config_volume: /srv/motion/config
      motion_log_volume: /srv/motion/log
      motion_data_volume: /srv/motion/data
      motion_cameras:
       - name: usb_cam
         stream_port: 8081
         target_dir: usb_cam
         options:
           videodevice: /dev/video0
           input: -1
           text_left: USB CAM
           width: 1920
           height: 1080
           framerate: 60
           ffmpeg_video_codec: mpeg4
       - name: video_card_cam
         stream_port: 8082
         options:
           videodevice: /dev/video1
           input: 1
           text_left: VIDEO CAM
```

## Links

* [Official Motion Documentation](https://motion-project.github.io/motion_config.html)
