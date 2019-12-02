## Run Over SSH

Run a shell command or script over ssh in one or more hosts.

### Installation

#### Debian/Ubuntu
```
sudo apt install runoverssh
```

#### Manually
```
sudo curl -L "https://raw.githubusercontent.com/yuriescl/runoverssh/master/runoverssh" -o /usr/local/bin/runoverssh && chmod +x /usr/local/bin/runoverssh
```

### Usage
```
$ runoverssh [OPTIONS] USERNAME COMMAND HOSTS...
```

### Default behavior

* SSH flags: `-o ConnectTimeout=5 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null`
* Uses `bash` as the remote shell

### Global Password

A global password can be used for all SSH connections.
It requires `sshpass` to be installed.  
See the `-g` flag.

### Options:
```
   -g, --globalpw             Prompt a global password for all connections
   -s, --script FILE          Read commands from a script file, disables
                               the default COMMAND argument
   -r, --hostsfile FILE       Read the list of hosts from a file (one host
                               per line), disables the default HOSTS argument
   -a, --args ARGS            Arguments (in a single string) to be passed to
                               the script file.
   -q, --quiet                Disable all screen output, except for password
                               prompts. If logfile is set, output is written
                               there
   -v, --verbose              Print verbose messages
   --shell SHELL              Remote shell to be used. Supported values:
                               sh, bash
                              default: bash
   --shellflags FLAGS         Remote shell flags
                              default: ''
   --sshflags FLAGS           Local SSH flags
                              default:  -o ConnectTimeout=5
                                        -o StrictHostKeyChecking=no
                                        -o UserKnownHostsFile=/dev/null
   --logfile FILE             Append SSH output to a file
```

### Examples
Restart Apache webserver in two hosts
```
runoverssh root "systemctl restart apache2" webserver webserver2
```
Reboot three hosts, which contain the same root password. Writes the SSH output to `reboot.log`.
```
runoverssh --logfile reboot.log --globalpw root "reboot" host1 host2 host3
```
Run puppet agent in all nodes listed in `puppet-nodes`, supressing the output
```
runoverssh -q -g -r puppet-nodes root "puppet agent -t"
```
Check git status on devmachine
```
runoverssh remoteuser "cd git-project && git status" devmachine
```
Run backup script in all hosts listed in a file
```
runoverssh -g --script backup.sh --hostsfile hostlist remoteuser
```
