# dotfiles

For a while now I've been using [dotbot](https://github.com/anishathalye/dotbot) to manage my dotfiles. While it works really well I sometimes found myself wanting something more powerful. At roughly the same time I started to use [Ansible](https://www.ansible.com/) to manage my servers, so I figured I could also use it to manage my dotfiles.

## Install

0. You have to install ansible. You can get a reasonably new version with `pip`.

1. To install this on a remote host you first have to manually enable (or just start for a single install) sshd. You can skip this step if you are just installing this on your local machine.
    ```sh
    sudo systemctl enable --now sshd
    ```

2. Add new host to `inventory` file. You can define as many hosts as you want (for example I have a desktop and a laptop)
    ```
    [hosts]
    desktop-address ansible_ssh_private_key_file=~/.ssh/desktop-key
    laptop-address ansible_ssh_private_key_file=~/.ssh/laptop-key
    #localhost ansible_connection=local # if you want to run this only on your local machine you can add your host like this to avoid starting sshd 

    [hosts:vars]
    ansible_become_password=correct_horse_battery_staple    # Since this file will be encrypted with ansible vault we can store sudo password here and avoid inputing it every time we run this
    ansible_user=vid
    ```

3. Edit variables in `vars.yml`

4. Run Ansible
    ```sh
    ansible-playbook main.yml --ask-vault-pass
    ```
    This directory will be copied to `~/dotfiles` (if not installed there already).

## Secret management

Secret files are encrypted with ansible-vault and are decrypted when copied to the correct location.
Most configuration files are symlinked but encrypted files have to be copied to the correct location.

## Customization

Currently this repo is really not well suited for anyone else than me. There are hard-coded values that should be variables all over the place.

