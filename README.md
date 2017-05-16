## Run Over SSH

This program runs a Bash command/script over ssh in one or more hosts.

Bugs or Requests: yuriescl@gmail.com

#### Usage:
```
$ run-over-ssh [OPTIONS] USERNAME COMMAND HOSTS...
```

#### Default behavior:

* Ask each `username@host` password at the start
* SSH connections use the flags `-o ConnectTimeout=5 -o StrictHostKeyChecking=no`
* Print all SSH output in the screen

#### Options:
```
     -s, --script [file]     read commands from a script file instead
     -r, --hostsfile [file]  use the list of hosts from a file (one host per line)
  
     -g, --globalpw          ask one global password for all connections
     -n, --nopw              no password (use ssh directly instead of sshpass)
     --sshflags [flags]      set custom ssh flags
                             default: '-o ConnectTimeout=5 -o StrictHostKeyChecking=no'
     -b, --bashflags         set custom bash flags
                             default: '-l'
  
     -l, --log               save ssh output (default: run-over-ssh.log) (overwrite)
     --logfile [file]        save ssh output to a custom file (overwrite)
     -q, --quiet             disable ssh screen output
  
```

#### Examples
```
$ run-over-ssh root "systemctl restart apache2" webserver webserver2
Please set root's password for each host:

root@webserver password: 
root@webserver2 password: 

Connecting as root@webserver...
Connecting as root@webserver2...
```
```
$ run-over-ssh --log --quiet --globalpw root "reboot" host1 host2 host3
root's password (used for all connections):

Connecting as root@host1...
Connecting as root@host2...
Connecting as root@host3...
```
```
$ run-over-ssh -q -l -g -r puppet-nodes root "puppet agent -t"
root's password (used for all connections):

Connecting as root@node1...
Connecting as root@node2...
Connecting as root@node3...
```
```
$ run-over-ssh remoteuser "cd git-project && git status" devmachine
Please set remoteuser's password for each host:

remoteuser@devmachine password: 

Connecting as remoteuser@devmachine...
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```
```
$ run-over-ssh -g --script backup.sh --hostsfile hostlist remoteuser
remoteuser's password (used for all connections):

Connecting as remoteuser@host1...
Backup successful
Connecting as remoteuser@host2...
Backup successful
Connecting as remoteuser@host3...
Backup successful
```
