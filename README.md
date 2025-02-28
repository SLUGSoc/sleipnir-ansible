# Sleipnir Ansible

This is a set of [Ansible](https://docs.ansible.com) Playbooks designed to secure Sleipnir's SSH server. While these Playbooks are intended specifically Sleipnir, they will work on pretty much any modern Debian-based server.

## Installation

### Cloning the Repo

The first step to using these Playbooks is to clone this repository. You can do this pretty easily with:

```shell
git clone https://github.com/SLUGSoc/sleipnir-ansible.git
```

### Installing Python

You'll need a relatively recent version of [Python](https://python.org) installed on your machine. 

### Installing Ansible

With the repository cloned and Python installed, the next step is to install Ansible. Ansible is just a set of Python packages, so you can install it with:

```shell
pip install -r requirements.txt
```

## Generating a Key-pair

You can generate a new SSH key-pair for your device by running this command:

```shell
ansible-playbook gen_key.yaml
```

This Playbook **runs on your local machine, not on the server**. This way, the private key never leaves your device (unless you choose to do so). Please see below for some tips on generating the most secure key-pair possible.

### Tips for Generating a Secure Key

In general, there are two factors that affect the innate security of an SSH key-pair: the algorithm used to generate it, and the length of the keys. By default, this repo will use the most secure options unless told to do otherwise.

See [this article](https://www.ssh.com/academy/ssh/keygen#choosing-an-algorithm-and-key-size) if you want to learn more about SSH key generation.

#### Generation Algorithm

There are various different algorithms available through OpenSSH when generating key-pairs. Each one is different in how they work, but generally newer algorithms are more secure.

**Note:** while one algorithm may support larger key lengths than another, it does not necessarily mean it's more secure. The algorithm choice has a greater impact on security than the choice of key length.

| Algorithm         | OpenSSH Support       | Released | Generation Method                           | Supported Lengths      |
|-------------------|-----------------------|----------|---------------------------------------------|------------------------|
| RSA               | Since inception       | 1977     | Difficulty of finding factors               | 1024, 2048, 3072, 4096 |
| DSA               | Deprecated since 2015 | 1991     | Difficulty of computing discrete logarithms | 1024                   |
| ECDSA             | Since 2011            | 1992     | Using properties of elliptic curves         | 256, 384, 521          |
| Ed25519 (default) | Since 2014            | 2011     | Using properties of elliptic curves         | 256                    |

#### Key Length

There are very few reasons to use a smaller key length, barring a *very* small impact to latency and login times. You should use the maximum key length supported by your generation algorithm of choice.

#### What about Hardware Security Keys?

If you have a hardware security key (e.g. YubiKey), you'll have to generate the key-pair yourself. This is because there's a number of different ways to do this, all with slight technical differences.

## Usage

![Skyrim Hadvar who are you?](https://i.reddituploads.com/fe7b542862744f68ab04aa907a5decec?fit=max&h=1536&w=1536&s=bc8e867615ad4093eaecdde2367a5323)

[I am a server admin](admin/README.md): you want to manage users and their public keys

[I am an ordinary user](user/README.md): you want to manage your own public keys

Depending on who you are and what you want to do, you'll either want the files in the [`admin`](admin/README.md) directory, or the files in the [`user`](user/README.md) directory. Please make sure to `cd` into your directory of choice before continuing.