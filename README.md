# Sleipnir Ansible

This is a set of [Ansible](https://docs.ansible.com) Playbooks designed to secure Sleipnir's SSH server. The Playbooks available are as follows:

- [`add_user.yaml`](/add_user.yaml): creates a new posix account with the given credentials
- [`del_user.yaml`](/del_user.yaml): deletes an existing posix account
- [`gen_key.yaml`](/gen_key.yaml): generates a new SSH key-pair on your local machine

While these Playbooks are intended specifically Sleipnir, they will work on pretty much any modern Debian-based server.

## Installing Ansible

To use these Playbooks, you must have Ansible installed on your machine (it doesn't have to be installed on your servers). Assuming you have a relatively recent version of [Python](https://python.org) installed, you can just do this with:

```shell
pip install -r requirements.txt
```

## Inventory Configuration

Playbooks that interact with a remote server must be configured with the Ansible Inventory at `inventory.yaml` (see [`example.inventory.yaml`](/example.inventory.yaml) for a template). The main variables of interest are:

- `ansible_host`: The address of the server to connect to and run the Playbook on.
- `ansible_port`: The port to use when SSHing into the server.
- `ansible_user`: The username that Ansible will login via SSH as on the server.
- `ansible_ssh_private_key_file`: Local path to the SSH private key used to connect to a server.
- `sudoers_group`: The posix group that gives users access to sudo.

If you are just using [`gen_key.yaml`](/gen_key.yaml) then you can just rename [`example.inventory.yaml`](/example.inventory.yaml) to `inventory.yaml`.

## Using a Playbook

### Generating an SSH Key-pair

```shell
ansible-playbook gen_key.yaml
```

When using this Playbook, you'll be asked to enter a variety of information about the key you want to generate. It will then create your new key-pair locally and store it at the path you specified (the public key will have the `.pub` extension appended to the path you gave).

The Playbook should make a decent attempt to guide you through the process and prevent you from generating faulty or insecure keys, or overwriting existing files. However, if you are still confused, then feel free to make an Issue on this repo, or contact Jack on Discord (username: `jacktek`).

### Creating a User

```shell
ansible-playbook add_user.yaml --ask-become-pass
```

This Playbook will ask for a username, password and whether the new user should have sudo access and then create that user. If a user already exists with the given username, then the only thing that will change is whether it has sudo access.

### Deleting a User

```shell
ansible-playbook del_user.yaml --ask-become-pass
```

This Playbook will ask for the username of a user to delete. If the user doesn't exist, nothing will happen.

## Tips for Generating a Secure Key

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