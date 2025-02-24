# Sleipnir Ansible: Admins

Hello! **This section is for Sleipnir's admins only**. If you're not an admin (i.e. you don't have `sudo` permissions), please [see this section for users](user/README.md).

## Inventory Configuration

All the Playbooks here require access to an account with `sudo` permissions. To use these Playbooks, you'll need to rename `example.inventory.yaml` to `inventory.yaml` and update the following variables:

- `ansible_host`: The address of the server to connect to and run the Playbook on.
- `ansible_port`: The port to use when SSHing into the server.
- `ansible_user`: The username that Ansible will login via SSH as on the server.
- `ansible_ssh_private_key_file`: Local path to the SSH private key used to connect to a server.
- `sudoers_group`: The posix group that gives users access to sudo.

## User Management

### Create a New User

```shell
ansible-playbook add_user.yaml --ask-become-pass
```

This asks for some basic user credentials and then creates a new user on the server. You can optionally give this new user `sudo` permissions.

After creating a new user, you'll need to [add a new public key](#add-a-public-key) to their account before they'll be able to login or manage their authorised keys.

### Delete an Existing User

```shell
ansible-playbook del_user.yaml --ask-become-pass
```

This will delete an existing user account. It won't allow deleting the `root` account (obviously), or the admin account that Ansible is using.

## Key Management

To avoid potentially bricking a server, none of these Playbooks will let you modify keys for the admin (or root) account Ansible is using. You can use the [user Playbooks](user/README.md) instead.

### Add a Public Key

```shell
ansible-playbook add_key.yaml --ask-become-pass
```

You'll be asked to provide the public key and the user to add it to. Alternatively, you can use a URL to a file containing public keys separated by new lines (GitHub exposes every user's public keys at `github.com/<username>.keys`).

Currently, there is no way to use a `.pub` file directly. Instead, you'll have to open (or `cat`) the file and copy-paste it into the console, sorry!

### Revoke a Public Key

```shell
ansible-playbook del_key.yaml --ask-become-pass
```

This will revoke a public key from a user's `authorized_keys`. Currently, you will need to specify the public key (minus the comment) exactly in order for Ansible to identify which one to remove. In future, I will try to add a way to identify public keys by their comment instead.

### List a User's Keys

```shell
ansible-playbook list_keys.yaml --ask-become-pass
```

This will list all of a user's authorised public keys. That's it, pretty self-explanatory.