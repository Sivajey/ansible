# Ansible:

# why?

The server systems are the underlying foundation of our applications and thus, like our applications, they should be version controlled, tested and automated. As such we looked for a configuration management tool that could solve our automation needs.This is where Ansible truly shines, it has lowered the learning curve enough so that it is easier to use Ansible than to do manual stuff or shell scripting, even for a local install and configuration.


# What?

Ansible uses no agents and no additional custom security infrastructure. Essentially, the desired state of the entire IT infrastructure can be described in the form of Ansible Playbooks, which are files written in a very simple language, YAML.

Ansible works by connecting to the nodes over SSH and pushing out small programs, called "Ansible Modules", to them. Ansible then executes and removes them when finished. Because these modules are simple Python scripts, and Ansible is agent-less, the target hosts only require an SSH connection and Python installed. Besides simplicity, which is another advantage of using Ansible vs Chef or Puppet, there are no servers, daemons, or databases required - all that is needed is to install Ansible on the host, Python on the targets, and have SSH access.

# How?

Ansible hosts are defined in a file, organised by groups and Playbooks can be applied to individual groups. The addresses of these hosts can be described using ranges and other operations. Finally, each host can have custom variables, such as SSH and sudo passwords.

Playbooks are the core of Ansible and the basis for a really simple configuration management and multi-machine deployment system. Playbooks can declare configurations, but they can also orchestrate steps of any manually ordered process, even as different steps must bounce back and forth between sets of machines in particular orders. They can launch tasks synchronously or asynchronously.

Playbooks are expressed in YAML format which makes them very simple and readable. Each playbook is composed of one or more plays in a list. The goal of a play is to map a group of hosts to some well-defined roles, represented by things Ansible calls tasks. At a basic level, a task is nothing more than a call to an Ansible module.

By composing a playbook with multiple plays, it is possible to orchestrate multi-machine deployments, running certain steps on all machines in the web servers group, then certain steps on the database server group, then more commands back on the web servers group, etc. 
Playbooks have the interesting characteristic of being idempotent, they ensure the system is in the desired state, and only run tasks if needed.


# Directory structure on Ansible role:

Defaults: The default variables for the role are been stored here inside this directory. These variables have the lowest priority.

Files: All the static files are being stored here which are used inside the role. 

Handlers: All the handlers are being used here not inside the Task directory. And automatically called upon from here.

Meta: This directory contains the metadata about your role regarding the dependencies which are being required to run this role in any system, so it will not be run until the dependencies inside it are not been resolved.

Tasks: This directory contains the main list of the tasks which needs to be executed by the role.

Vars: This directory has high precedence than defaults directory and can only be overwritten by passing them On the command line, In the specific task or In a block.

Templates: This directory contains the Jinja to template inside this. Basically, all the dynamic files are being stored here which can be variablized.


# Best Practices:

Whitespace and Comments:
Generous use of whitespace and breaking things up is really appreciated. One very important thing is the use of comments inside your roles so that someone using your role in future could be able to easily understand it properly.

YAML format:
Learn YAML format properly and use of indentation properly inside the document. Sometimes, when running the role gives the error for Invalid Syntax due to bad indentation format. And writing in proper Indentation makes your role look beautiful.

Always Name Tasks:
It is possible to leave off the ‘name’ for a given task, though it is recommended to provide a description about something is being done instead. This name is shown when that particular task is being run.

Version Control:
Use version control. Keep your roles and inventory files in git and commit when you make changes to them. This way you have an audit trail describing when and why you changed the rules that are automating your infrastructure. 

Variable and Vaults:
Since the variable contains sensitive data, so It is often easier to find variables using grep or similar tools inside the Ansible system. Since vaults obscure these variables, It is best to work with a layer of Indirection. This allows Ansible to find the variables inside the unencrypted file and all sensitive variables come from an encrypted file.
The best approach to perform is to start with a group_vars subdirectory containing two more subdirectories inside it naming “Vars” and “Vaults”. Inside “Vars”  directory define all the variable including sensitive variables also. Now, copy those sensitive variables inside “Vault” directory while using the prefix “vault_*” for the variables. Now you should adjust the variables in the “Vars” to point the matching “vault_*” variables using jinja2 syntax and ensure that vault file is vault encrypted.

Roles for multiple OS:
Roles should be written in a way that they could be run on multiple Operating systems. Try to make your roles as generic as you can. But if you have created a role for some specific kind of operating system or some specific application, then try to explicitly define that inside the role name.

Single role Single goal:
Avoid tasks within a role which are not related to each other. Don’t build a common role. It’s ugly and bad for readability of your role.


Ansible-Vault:

Having infrastructure as code can pose the threat of exposing your sensitive data to the world, leading to unwanted security issues. Ansible Vault is a feature that allows you to keep all your secrets safe. It can encrypt entire files, entire YAML playbooks or even a few variables. It provides a facility where you can not only encrypt sensitive data but also integrate them into your playbooks.

Vault is implemented with file-level granularity where the files are either entirely encrypted or entirely unencrypted. It uses the same password for encrypting as well as for decrypting files which makes using Ansible Vault very user-friendly.

Creating a secret file:

ansible-vault create secrets.txt

You’ll be prompted to create a password and then confirm it by re-typing it.Then You can add data, save and exit. 

Encrypting specific variables: 

Best practice while using Ansible Vault is to encrypt only the sensitive data. In the example explained above, the development team does not want to share their password with the production and the staging team but they might need access to certain data to carry out their own task. In such cases you should only be encrypting the data you do not want to share with others, leaving the rest as it is. 

ansible-vault create vault.yml

add a new variable vault_new_secret = "your secret"  
Save and exit.

Importing the vault file:

vim ansible/vars/vars.yml

new_secret: "{{ vault_new_secret }}"


Running the  ansible-playbook:

git clone https://github.com/Sivajey/ansible.git

ansible-playbook --ask-become-pass site.yml --ask-vault-pass 


