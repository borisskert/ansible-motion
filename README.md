# ansible-motion

Installs motion as a systemd-managed docker container.

## Requirements

### CPU architecture:

* x86_64 / amd64
* armv7l / armhf
* aarch64 / arm64

### Operating systems:

* Ubuntu:
  * 16.04 (xenial)
  * 18.04 (bionic)
  * 20.04 (focal)
* Debian
  * 9 (stretch)
  * 10 (buster)

## Role requirements

* python-docker package

## What this role does

* Create volume directories
* Setup motion config
* Setup motion cam configs
* Setup Dockerfile and build docker image
* Setup systemd unit file
* Restart/start systemd service

## Role parameters

| Variable       | Type | Mandatory? | Default | Description                  |
|----------------|------|------------|---------|------------------------------|
| motion_config_volume | text | yes  |         | Where the motion config will be stored |
| motion_log_volume    | text | yes  |         | Where the motion logs will be stored   |
| motion_data_volume   | text | yes  |         | Where the motion data (like records) will be stored |
| motion_cameras       | array of `motion_camera` | no | `[]` | The separate camera configurations |

### `motion_camera` definition

| Variable | Type | Mandatory? | Default | Description                  |
|----------|------|------------|---------|------------------------------|
| name     | text | yes        |         | Configured camera name; Used for the configuration file name |
| stream_port | integer | yes  |         | The configured cam stream port |
| target_dir  | text    | no   |         | The directory to store the records of this cam |
| options     | Map of text key/values | no | `[]` | The motion camera options [Read the docs!](https://motion-project.github.io/motion_config.html) |

## Usage

### Add to `requirements.yml`

```yaml
- name: install-motion
  src: https://github.com/borisskert/ansible-motion.git
  scm: git
```

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

## Testing

Requirements:

* [Vagrant](https://www.vagrantup.com/)
* [VirtualBox](https://www.virtualbox.org/)
* [Ansible](https://docs.ansible.com/)
* [Molecule](https://molecule.readthedocs.io/en/latest/index.html)
* [yamllint](https://yamllint.readthedocs.io/en/stable/#)
* [ansible-lint](https://docs.ansible.com/ansible-lint/)
* [Docker](https://docs.docker.com/)

### Run within docker

```shell script
molecule test
```

### Run within Vagrant

```shell script
 molecule test --scenario-name vagrant --parallel
```

I recommend to use [pyenv](https://github.com/pyenv/pyenv) for local testing.
Within the Github Actions pipeline I use [my own molecule Docker image](https://github.com/borisskert/docker-molecule).

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)

## Links

* [Official Motion Documentation](https://motion-project.github.io/motion_config.html)
