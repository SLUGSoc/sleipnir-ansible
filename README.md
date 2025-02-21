# Sleipnir Ansible

This is a set of [Ansible](https://docs.ansible.com) Playbooks designed to secure Sleipnir's SSH server. The Playbooks available are as follows:

- [`add_user.yaml`][/add_user.yaml]: creates a new posix account with the given credentials
- [`del_user.yaml`][/del_user.yaml]: deletes an existing posix account

## Installing Ansible

To use these Playbooks, you must have Ansible installed on your machine (it doesn't have to be installed on your servers). Assuming you have a relatively recent version of [Python](https://python.org) installed, you can just do this with:

```shell
pip install -r requirements.txt
```

## Configuration

All the Playbooks in this repo can be configured with the Ansible Inventory at `inventory.yaml` (see [`example.inventory.yaml`](/example.inventory.yaml) for a template). The main variables of interest are:

- `ansible_host`: The address of the server to connect to and run the Playbook on.
- `ansible_port`: The port to use when SSHing into the server.
- `ansible_user`: The username that Ansible will login via SSH as on the server.
- `key_algorithm`: The algorithm used to generate key-pairs. In general, `ed25519` is the most secure option on modern systems.
- `key_size`: The size of the key to generate. See below for lengths supported be your `key_algorithm`.

## Using a Playbook

### Creating a User

```shell
ansible-playbook add_user.yaml --ask-become-pass
```

This Playbook will create a new user with a given username & password.

### Deleting a User

```shell
ansible-playbook del_user.yaml --ask-become-pass
```

This Playbook will list all the users on the server and delete the chosen one.

## Choosing a Secure Key

In general, there are two factors that affect the innate security of an SSH key-pair: the algorithm used to generate it, and the length of the keys. By default, this repo will use the most secure options unless told to do otherwise.

See [this article](https://www.ssh.com/academy/ssh/keygen#choosing-an-algorithm-and-key-size) if you want to learn more about SSH key generation.

### Generation Algorithm

There are various different algorithms available through OpenSSH when generating key-pairs. Each one is different in how they work, but generally newer algorithms are more secure.

**Note:** while one algorithm may support larger key lengths than another, it does not necessarily mean it's more secure. The algorithm choice has a greater impact on security than the choice of key length.

| Algorithm         | OpenSSH Support       | Released | Generation Method                           | Supported Lengths      |
|-------------------|-----------------------|----------|---------------------------------------------|------------------------|
| RSA               | Since inception       | 1977     | Difficulty of finding factors               | 1024, 2048, 3072, 4096 |
| DSA               | Deprecated since 2015 | 1991     | Difficulty of computing discrete logarithms | 1024                   |
| ECDSA             | Since 2011            | 1992     | Using properties of elliptic curves         | 256, 384, 521          |
| Ed25519 (default) | Since 2014            | 2011     | Using properties of elliptic curves         | 256                    |

### Key Length

There are very few reasons to use a smaller key length, barring a *very* small impact to latency and login times. You should use the maximum key length supported by your generation algorithm of choice.