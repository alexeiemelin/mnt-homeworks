```bash
ansible-playbook -i inventory/prod.yml site.yml --check -vvv
ansible-playbook [core 2.12.1]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/user/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/user/.local/lib/python3.8/site-packages/ansible
  ansible collection location = /home/user/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/user/.local/bin/ansible-playbook
  python version = 3.8.10 (default, Mar 15 2022, 12:22:08) [GCC 9.4.0]
  jinja version = 3.1.2
  libyaml = True
Using /etc/ansible/ansible.cfg as config file
host_list declined parsing /home/user/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook/inventory/prod.yml as it did not pass its verify_file() method
script declined parsing /home/user/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook/inventory/prod.yml as it did not pass its verify_file() method
Parsed /home/user/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook/inventory/prod.yml inventory source with yaml plugin
ERROR! 'pre-tasks' is not a valid attribute for a Play

The error appears to be in '/home/user/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook/site.yml': line 101, column 3, but may
be elsewhere in the file depending on the exact syntax problem.

The offending line appears to be:


- name: Install Lighthouse
  ^ here
```