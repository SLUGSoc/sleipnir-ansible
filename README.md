# Sleipnir Ansible

This is a set of [Ansible](https://docs.ansible.com) Playbooks designed to secure Sleipnir's SSH server. While these Playbooks are intended specifically Sleipnir, they will work on pretty much any modern Debian-based server.

## Installing Ansible

To use these Playbooks, you must have Ansible installed on your machine (it doesn't have to be installed on your servers). Assuming you have a relatively recent version of [Python](https://python.org) installed, you can just do this with:

```shell
pip install -r requirements.txt
```

## Inventory Configuration

Playbooks that interact with a remote server must be configured with the Ansible Inventory at `inventory.yaml` (see [`example.inventory.yaml`](/admin/example.inventory.yaml) for a template). The main variables of interest are:

- `ansible_host`: The address of the server to connect to and run the Playbook on.
- `ansible_port`: The port to use when SSHing into the server.
- `ansible_user`: The username that Ansible will login via SSH as on the server.
- `ansible_ssh_private_key_file`: Local path to the SSH private key used to connect to a server.
- `sudoers_group`: The posix group that gives users access to sudo.