# Sleipnir Ansible

This is a set of [Ansible](https://docs.ansible.com) Playbooks designed to secure Sleipnir's SSH server. While these Playbooks are intended specifically Sleipnir, they will work on pretty much any modern Debian-based server.

## Installing Ansible

To use these Playbooks, you must have Ansible installed on your machine (it doesn't have to be installed on your servers). Assuming you have a relatively recent version of [Python](https://python.org) installed, you can just do this with:

```shell
pip install -r requirements.txt
```

## Who are you?

![Skyrim Hadvar who are you?](https://i.reddituploads.com/fe7b542862744f68ab04aa907a5decec?fit=max&h=1536&w=1536&s=bc8e867615ad4093eaecdde2367a5323)

[I am a server admin](admin/README.md): you want to manage users and their public keys

[I am an ordinary user](user/README.md): you want to manage your own public keys
