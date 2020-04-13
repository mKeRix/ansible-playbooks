# Ansible Playbooks

## Installation

To use the playbooks you need to have a current version of [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html). Additionally, you should run `ansible-galaxy install -r requirements.yml` in your cloned repository folder once to download the dependencies.

## Playbooks

### room-assistant

The `room-assistant.yml` playbook allows you to install and manage a cluster of [room-assistant](https://github.com/mkerix/room-assistant) installations. For this you need to create an [inventory file](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html) with the hosts you want to manage and their configuration. Below is an example for such a `hosts.yml` file.

```yaml
all:
  hosts:
    'living-room.local':
      ansible_user: pi
      ansible_password: raspberry
      room_assistant_config: 
        global:
          integrations:
            - homeAssistant
            - bluetoothClassic
            - omronD6t
            - gpio
        gpio:
          binarySensors:
            - name: Motion Sensor
              pin: 17
              deviceClass: motion
    'bedroom.local':
      ansible_user: pi
      ansible_password: raspberry
  vars:
    room_assistant_global_config:
      global:
        integrations:
          - homeAssistant
          - bluetoothClassic
      homeAssistant:
        mqttUrl: mqtt://hassio.local:1883
        mqttOptions:
          username: room-assistant
          password: secretpass
      bluetoothClassic:
        addresses:
          - 'xx:xx:xx:xx:xx:xx'
```

Note that instead of using user/password you can also use public key authentication if you previously setup your Pis to allow this.

#### Options

| Variable                       | Default  | Description                                                  |
| ------------------------------ | -------- | ------------------------------------------------------------ |
| `room_assistant_version`       | `latest` | The version of room-assistant to install, as denoted on [npm](https://www.npmjs.com/package/room-assistant). |
| `room_assistant_global_config` |          | A room-assistant config that should be used across all instances. |
| `room_assistant_config`        |          | A room-assistant config for specific hosts. Options that are also in the global config will be overridden. |

