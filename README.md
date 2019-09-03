## Run Over SSH

This program runs a Bash command/script over ssh in one or more hosts.

### Installation

#### Ubuntu
```
sudo apt install runoverssh
```

#### Manually
```
git clone https://github.com/yuriescl/runoverssh
sudo cp runoverssh/runoverssh /usr/local/bin/runoverssh
rm -rf runoverssh
```

### Usage
```
$ runoverssh [OPTIONS] USERNAME COMMAND HOSTS...
```

### Default behavior

* Ask each `username@host` password at the start
* SSH flags `-o ConnectTimeout=5 -o StrictHostKeyChecking=no`
* Bash flags `-l`
* Prints all SSH output in the screen

### SSH Authentication

`sshpass` is used for authenticating using a password.  
For using SSH directly (no `sshpass`), use the `--nopw`/`-n` flag.
This is useful for SSH public-key authentication.

### Options:
```
  -g, --globalpw             ask one global password for all connections
  -s, --script [file]        read commands from a script file instead
  -r, --hostsfile [file]     use the list of hosts from a file (one host per line)
  
  -n, --nopw                 no password (use ssh directly instead of sshpass)
  -a, --args                 specify the arguments to be passed to the script file
  -l, --log                  save ssh output (default: runoverssh.log) (append)
  -q, --quiet                disable ssh screen output
  
  --bashflags [flags]        set custom bash flags
                              default: '-l'
  --sshflags [flags]         set custom ssh flags
                              default: '-o ConnectTimeout=5 -o StrictHostKeyChecking=no'
  --logfile [file]           save ssh output to a custom file (append)

```

### Examples
```
# Restart Apache webserver in two hosts
$ runoverssh root "systemctl restart apache2" webserver webserver2
Please set root's password for each host:

root@webserver password: 
root@webserver2 password: 

Connecting as root@webserver...
Connecting as root@webserver2...
```
```
# Reboot three hosts, which contain the same root password. Log the output.
$ runoverssh --log --globalpw root "reboot" host1 host2 host3
root's password (used for all connections):

Connecting as root@host1...
Connecting as root@host2...
Connecting as root@host3...
```
```
# Run puppet agent in all nodes listed in a file supressing output from ssh.
$ runoverssh -q -l -g -r puppet-nodes root "puppet agent -t"
root's password (used for all connections):

Connecting as root@node1...
Connecting as root@node2...
Connecting as root@node3...
```
```
# Check git status on devmachine
$ runoverssh remoteuser "cd git-project && git status" devmachine
Please set remoteuser's password for each host:

remoteuser@devmachine password: 

Connecting as remoteuser@devmachine...
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```
```
# Run backup script in all hosts listed in a file
$ runoverssh -g --script backup.sh --hostsfile hostlist remoteuser
remoteuser's password (used for all connections):

Connecting as remoteuser@host1...
Backup successful
Connecting as remoteuser@host2...
Backup successful
Connecting as remoteuser@host3...
Backup successful
```
