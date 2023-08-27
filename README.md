# Summary
This are my ansible configs to create and install roles in remote machines. I use `ansible-pull` command to provision each host.

## Install
Before the host can run by itself you must push the ansible configuration only once using the command:
```bash
ansible-playbook all.yaml -u root --ask-pass -i $hostname,
```
The file `all.yaml` is the playbook file containeing the initial hosts who will be running `ansible-pull` job.
An example of this file can be:
```yaml
- hosts: all
  gather_facts: yes
  roles: 
    - common
```
### Configuring timer
To configure the timer you can specify when the `ansible-pull` service can run you can pass the variable `ansible_pull_timer`. This variable accepts the systemd `[timer]` format. Example:
```bash
ansible-playbook all.yaml -u root --ask-pass -i $hostname, --extra-vars '{"ansible_pull_timer":"OnBootSec=5min\nOnCalendar=daily"}'
```

### Ansible-pull options
You can modify the variables in the `roles/common/defaults/main.yaml` path to change the `ansible-pull` options:
| Variable             | Description                                                                        | Default value                              |
|----------------------|------------------------------------------------------------------------------------|--------------------------------------------|
| only_if_change       | Run playbook only if the repository has changed                                    | `false`                                    |
| ansible_pull_timer   | Configure the `[timer]` block                                                      | `OnBootSec=5min<br>OnCalendar=daily`       |
| repo_url             | Repository url                                                                     | `https://github.com/elohim18bolts/ansible` |
| systemd_dir          | Directory where the `ansible-pull.timer` and `ansible-pull.service` will be copied | `/etc/systemd/system`                      |
| repo_local_directory | Directory where the repo will be cloned                                            | `/opt/ansible`                             |
| playbook_path        | Playbook file path                                                                 | `/opt/ansible/play.yaml`                   |
| log_file_path        | Path where the output of the `ansible-pull` action will be stored                  | `/var/log/ansible-pull.log`                |
|----------------------|------------------------------------------------------------------------------------|--------------------------------------------|

