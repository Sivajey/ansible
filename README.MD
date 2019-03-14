Ansibe roles tutorial:
Ansible roles is a structured way of grouping tasks, handlers, vars and other properties. They increase reusability. You can easily share roles through Ansible Galaxy.

Commands:

ansible all -m ping


Using the ansible-galaxy command line tool that comes bundled with Ansible, you can create a role with the init command

ansible-galaxy init apache


Syntax check:

ansible-playbook site.yml --syntax-check

Final command:

ansible-playbook --ask-become-pass site.yml
