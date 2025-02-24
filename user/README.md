# Sleipnir Ansible: Users

This section is for users wanting to manage their own accounts. If you need to modify other users, [see the section for admins](admin/README.md).

## Inventory Configuration

These Playbooks will work regardless of whether you have `sudo` permissions or not. Your account will need to already have a public key authorised on the server, and the private key available for Ansible to use. If this is not the case, please contact a server admin.

To use these Playbooks, you'll need to rename `example.inventory.yaml` to `inventory.yaml` and update the following variables:

- `ansible_host`: The address of the server to connect to and run the Playbook on.
- `ansible_port`: The port to use when SSHing into the server.
- `ansible_user`: The username that Ansible will login via SSH as on the server.
- `ansible_ssh_private_key_file`: Local path to the SSH private key used to connect to a server.

## Generating a Key-pair

```shell
ansible-playbook gen_key.yaml
```

This will ask you a few questions and then generate a private-public key-pair on your device. You can then [add the public key to your account](#adding-a-key) so that it can be used for logging into Sleipnir.

This Playbook **runs on your local machine, not on the server**. This way, the private key never leaves your device (unless you choose to do so). Please see [here](#tips-for-generating-a-secure-key) for some tips on generating the most secure key-pair possible.

## Adding a Key

```shell
ansible-playbook add_key.yaml
```

This will ask you for a public (NOT private) key and then add it to your account's `authorized_keys` file. 

When asked for a comment, please give something that accurately identifies the device the key-pair is for. For example, the key for your MacBook might be commented with `macbook`, and the key for your Windows desktop might be commented `desktop`. This isn't an exact science, but it should allow you to easily identify the device storing the private key that matches the public key.

## Revoking a Key

```shell
ansible-playbook del_key.yaml
```

As with adding a public key, this Playbook will ask for a key (minus the comment) to remove from your `authorized_keys`. In the future, I hope to add a system that lets you remove keys just by giving the comment.

**Note:** please be very careful not to revoke the public key that matches the private key you're using with Ansible, or you will lock yourself out. I hope to detect this automatically in the future, but for now Ansible won't stop you from doing so.

## Listing your Keys

```shell
ansible-playbook list_keys.yaml
```

This will list all the public keys in your `authorized_keys` file. This could be helpful when you want to revoke a key without having to login and open the file manually to check which one it is (which is a bit tedious).

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