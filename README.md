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
          device: /dev/video0
          stream_port: 8082
          input: -1
          text_left: USB CAM
        - name: video_card_cam
          device: /dev/video1
          stream_port: 8083
          input: 1
          text_left: VIDEO CAM
```
