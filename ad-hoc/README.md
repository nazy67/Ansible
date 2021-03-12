## Ad-hoc commands:

### Task_1

1. Find all distribution related information of managed hosts.

ansible -i /home/ansible/myhosts.yaml all -m setup -a "filter=ansible_dist*"

2. Add user anna, with uid 2040 and comment “System Engineer” to all managed servers.

ansible -i /home/ansible/myhosts.yaml all -m user -a "name=anna uid=2040 comment=SystemEngineer"

3. Create an empty file called “ansible-testfile” in /tmp folder of all managed servers.

ansible -i /home/ansible/myhosts.yaml all -m file -a "path=/tmp/ansible-testfile state=touch"

4. Modify user anna’s uid to 2050 on all managed servers.

ansible -i /home/ansible/myhosts.yaml all -m user -a "name=anna uid=2050"

5. Check “free disk” on all managed servers, and save the output to /tmp/diskusage.

ansible -i /home/ansible/myhosts.yaml all -m shell -a "df -h" >>  /tmp/diskusage

6.  Check free memory of group “remote-hosts” and save the output to /tmp/freememory.

ansible -i /home/ansible/myhosts.yaml remote_hosts -m shell -a "free -h" >>  /tmp/freememory

7.  Check the /tmp folder if those files.

ansible -i /home/ansible/myhosts.yaml localhost -m shell -a "ls /tmp"
