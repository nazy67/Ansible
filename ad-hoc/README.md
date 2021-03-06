## Ad-hoc commands:

### Task_1

1. Find all distribution related information of managed hosts.
```
ansible -i /home/ansible/myhosts.yaml all -m setup -a "filter=ansible_dist*"
```
2. Add user anna, with uid 2040 and comment “System Engineer” to all managed servers.
```
ansible -i /home/ansible/myhosts.yaml all -m user -a "name=anna uid=2040 comment=SystemEngineer"
```
3. Create an empty file called “ansible-testfile” in /tmp folder of all managed servers.
```
ansible -i /home/ansible/myhosts.yaml all -m file -a "path=/tmp/ansible-testfile state=touch"
```
4. Modify user anna’s uid to 2050 on all managed servers.
```
ansible -i /home/ansible/myhosts.yaml all -m user -a "name=anna uid=2050"
```
5. Check “free disk” on all managed servers, and save the output to /tmp/diskusage.
```
ansible -i /home/ansible/myhosts.yaml all -m shell -a "df -h" >>  /tmp/diskusage
```
6.  Check free memory of group “remote-hosts” and save the output to /tmp/freememory.
```
ansible -i /home/ansible/myhosts.yaml remote_hosts -m shell -a "free -h" >>  /tmp/freememory
```
7.  Check the /tmp folder if those files.
```
ansible -i /home/ansible/myhosts.yaml localhost -m shell -a "ls /tmp"
```
### Task_2

1. Run ansible ad-hoc command and ensure ansible-server can ping the newly added hosts
```
ansible -i inventory.yaml all -m ping
```
Output:
```
    host1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
}
host2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
}
```
2. Run ansible ad-hoc command to find the distribution of newly added hosts 
```
ansible -i inventory.yaml dev_servers -m setup -a "filter=ansible_dist*"
```
Output:
```
host2 | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "CentOS", 
        "ansible_distribution_file_parsed": true, 
        "ansible_distribution_file_path": "/etc/redhat-release", 
        "ansible_distribution_file_variety": "RedHat", 
        "ansible_distribution_major_version": "7", 
        "ansible_distribution_release": "Core", 
        "ansible_distribution_version": "7.8", 
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false
}
host1 | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "CentOS", 
        "ansible_distribution_file_parsed": true, 
        "ansible_distribution_file_path": "/etc/redhat-release", 
        "ansible_distribution_file_variety": "RedHat", 
        "ansible_distribution_major_version": "7", 
        "ansible_distribution_release": "Core", 
        "ansible_distribution_version": "7.8", 
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false
}
```
3. Run ansible ad-hoc command to check the free memory of newly added hosts 
```
ansible -i inventory.yaml dev_servers -m shell -a "free -h"
```
Output:
```
host2 | CHANGED | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:           991M        105M        742M         12M        143M        738M
Swap:            0B          0B          0B
host1 | CHANGED | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:           991M        104M        742M         12M        144M        738M
Swap:            0B          0B          0B

```
4. Run ansible ad-hoc command to check storage available in newly added hosts 
```
ansible -i inventory.yaml dev_servers -m shell -a "df -h"
```
Output:
```
host2 | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        473M     0  473M   0% /dev
tmpfs           496M     0  496M   0% /dev/shm
tmpfs           496M   13M  483M   3% /run
tmpfs           496M     0  496M   0% /sys/fs/cgroup
/dev/vda1        25G  839M   25G   4% /
tmpfs           100M     0  100M   0% /run/user/0
host1 | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        473M     0  473M   0% /dev
tmpfs           496M     0  496M   0% /dev/shm
tmpfs           496M   13M  483M   3% /run
tmpfs           496M     0  496M   0% /sys/fs/cgroup
/dev/vda1        25G  839M   25G   4% /
tmpfs           100M     0  100M   0% /run/user/0
```
### Task_3

1. Run ad-hoc command to check free memory on web host
```
ansible -i poc_servers.yaml web -m shell -a "free -h"
```
Output:
```
web | CHANGED | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:          981Mi       270Mi        98Mi       0.0Ki       612Mi       531Mi
Swap:            0B          0B          0B
```
2. Run ad-hoc command to check free disk on we host 
``` 
ansible -i poc_servers.yaml web -m shell -a "df -h"
```
Output:
```
 web | CHANGED | rc=0 >>
 Filesystem      Size  Used Avail Use% Mounted on
 udev            474M     0  474M   0% /dev
 tmpfs            99M  956K   98M   1% /run
 /dev/vda1        25G  1.9G   23G   8% /
 tmpfs           491M     0  491M   0% /dev/shm
 tmpfs           5.0M     0  5.0M   0% /run/lock
 tmpfs           491M     0  491M   0% /sys/fs/cgroup
 /dev/vda15      105M  3.9M  101M   4% /boot/efi
 /dev/loop0       56M   56M     0 100% /snap/core18/1944
 /dev/loop1       56M   56M     0 100% /snap/core18/1885
 /dev/loop2       71M   71M     0 100% /snap/lxd/16922
 /dev/loop3       70M   70M     0 100% /snap/lxd/19032
 /dev/loop4       31M   31M     0 100% /snap/snapd/9607
 /dev/loop5       32M   32M     0 100% /snap/snapd/10707
 tmpfs            99M     0   99M   0% /run/user/0
``` 
3. Run ad-hoc command to check the status of nginx service
```
ansible -i poc_servers.yaml all -m shell -a "systemctl status nginx" 
```
Output:
```
web | CHANGED | rc=0 >>
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2021-01-31 00:31:04 UTC; 38min ago
     Docs: man:nginx(8)
  Process: 1897 ExecStop=/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.pid (code=exited, status=0/SUCCESS)
  Process: 1951 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
  Process: 1942 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
 Main PID: 1955 (nginx)
    Tasks: 2 (limit: 1152)
   CGroup: /system.slice/nginx.service
           ├─1955 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─1959 nginx: worker process

Jan 31 00:31:04 ubuntu systemd[1]: Starting A high performance web server and a reverse proxy server...
Jan 31 00:31:04 ubuntu systemd[1]: nginx.service: Failed to parse PID from file /run/nginx.pid: Invalid argument
Jan 31 00:31:04 ubuntu systemd[1]: Started A high performance web server and a reverse proxy server.
```
4. Run ad-hoc command to check distribution of web host (hint  use setup module)
```
ansible -i poc_servers.yaml web -m setup -a "filter=ansible_distr*"
```
Output:
```
web | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu", 
        "ansible_distribution_file_parsed": true, 
        "ansible_distribution_file_path": "/etc/os-release", 
        "ansible_distribution_file_variety": "Debian", 
        "ansible_distribution_major_version": "20", 
        "ansible_distribution_release": "focal", 
        "ansible_distribution_version": "20.04", 
        "discovered_interpreter_python": "/usr/bin/python3"
    }, 
    "changed": false
}
```  
5. Run ad-hoc command to check hostname on web host (bin use setup module)
```
ansible -i poc_servers.yaml web -m setup -a "filter=ansible_nodename"
```
Output:
```
web | SUCCESS => {
    "ansible_facts": {
        "ansible_nodename": "ubuntu", 
        "discovered_interpreter_python": "/usr/bin/python3"
    }, 
    "changed": false
}
```
