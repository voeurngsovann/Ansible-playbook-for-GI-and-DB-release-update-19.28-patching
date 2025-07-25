Instruction of using this ansible playbook for patch GI and DB 19.28 and other oneoffpatch 
go to GIRU folder and run 
ansible-playbook -i hosts apply_grid_update_main.yml -vvv
go to DBRU folder and run 
ansible-playbook -i hosts apply_db_update_main.yml -vvv
the expectation in your host should have client servers

the expectation and requirement  in your ansible servers to client servers need to configure passwordless both root, oracle and grid user 
Adjust patch directory in your clients  and source_patch_dir in your ansible server accourdingly based on space available , example  
patch_dir: /soft
source_patch_dir: /installer/software/DB28 
[ansible@ansible-server]$ cat hosts
[dbservers]
dbserver-01
dbserver-02

[ansible@ansible-server]$ ansible all -i hosts -k -u root -m authorized_key -a "user=root state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""
[ansible@ansible-server]$ ansible all -i hosts -k -u grid -m authorized_key -a "user=oracle state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""
[ansible@ansible-server]$ ansible all -i hosts -k -u oracle -m authorized_key -a "user=oracle state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""

 
