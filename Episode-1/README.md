## Requirements 

### On you local machine where Ansible will run

- Install via Python Packet manager: `pip3 install ansible`
- Check installation: `ansible --version`

### On the server 

- The server must have the following requirements:
  - SSH server and 
  - Python3
  - User with sudo privileges

### SSH keys

- Generation of keys
  - Create pair of keys with : `ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/store/path -C admin@email.com`
  - Copy public key with : `ssh-copy-id -i ~/.ssh/store/path user@your-server`
  - Check connecting to your server with : `ssh -i ~/.ssh/store/path user@your-server`
  - You must **not** be prompted with a password.

## Create an inventory

- Create a file named `inventory` and put the following content, ajust to your config :

```
[group_name]
172.16.0.10
```

## Check ansible can connect to the server

- Using the ping module, we can test the connection: `ansible -i [inventory_file] [Name_Of_Group] -m ping -u [user]`
  - `-i [inventory_file] [Name_Of_Group]` specifies the inventory file and the group used.
  - `-m` use the `ping` module.
  - `-u [user]` specifies the user which you connect to the server.
  - Example: `ansible -i inventory homeserver -m ping -u ansible`

**The above command must run in the same inventory as the inventory file.**

If everything is ok, you shoud get :

```
172.16.0.10 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

## Adding a config file

- You can add a config file in the root directory of your projet.
- Create a file named `ansible.cfg`
- Put extra config like so :

```
[defaults]
INVENTORY = Inventory
```

- Now try `ansible homeserver -m ping -u ansible`
- You should get the same result.

## Connecting without ssh keys

- You can specify a password using the option `-K` or `--ask-pass`.

## Start using usefull commands

### Using AddHoc with `-a`

- Check the disk available space : `ansible homeserver -a "free -h" -u ansible`

```
172.16.0.10 | CHANGED | rc=0 >>
               total        used        free      shared  buff/cache   available
Mem:           976Mi        72Mi       557Mi       0.0Ki       346Mi       765Mi
Swap:             0B          0B          0B
```

- Check date `ansible homeserver -a "date" -u ansible`

```
172.16.0.10 | CHANGED | rc=0 >>
Tue Mar 15 18:06:06 UTC 2022
```