# Ansible-Playbooks-Patching-GI-DB--19.28 and other oneoffpatch
This is the Ansible Playbooks for Patching Oracle RAC both Grid and Database Release Update 19.28
# prerequisite
  you’ll need to download the following setup files from Oracle Support and upload in ansible server
 - /installer/software/GI28
 - /installer/software/DB28
# configure ansible passwordless to targets servers for root,grid and oracle user 
  - [ansible@ansible-server giru]$ cat hosts 

       [dbservers]
      dev-dbserver01.localdomain    ansible_python_interpreter=/usr/bin/python3.6
      dev-dbserver02.localdomain    ansible_python_interpreter=/usr/bin/python3.6

# configure root passwordless

[ansible@ansible-server giru]$ansible all -i hosts -k -u root -m authorized_key -a "user=root state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""

# configure grid passwordless

[ansible@ansible-server giru]$ ansible all -i hosts -k -u grid -m authorized_key -a "user=grid state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""


# configure oracle passwordless

[ansible@ansible-server giru]$ ansible all -i hosts -k -u oracle -m authorized_key -a "user=oracle state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""

# running using command  for patchng GI
ansible-playbook -i hosts apply_grid_update_main.yml -vvv
# running using command  for patchng DB
ansible-playbook -i hosts apply_db_update_main.yml -vvv
