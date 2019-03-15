Ansible:

why?

The server systems are the underlying foundation of our applications and thus, like our applications, they should be version controlled, tested and automated. As such we looked for a configuration management tool that could solve our automation needs.This is where Ansible truly shines, it has lowered the learning curve enough so that it is easier to use Ansible than to do manual stuff or shell scripting, even for a local install and configuration.


What?

Ansible uses no agents and no additional custom security infrastructure. Essentially, the desired state of the entire IT infrastructure can be described in the form of Ansible Playbooks, which are files written in a very simple language, YAML.

Ansible works by connecting to the nodes over SSH and pushing out small programs, called "Ansible Modules", to them. Ansible then executes and removes them when finished. Because these modules are simple Python scripts, and Ansible is agent-less, the target hosts only require an SSH connection and Python installed. Besides simplicity, which is another advantage of using Ansible vs Chef or Puppet, there are no servers, daemons, or databases required - all that is needed is to install Ansible on the host, Python on the targets, and have SSH access.

How?

Ansible hosts are defined in a file, organised by groups and Playbooks can be applied to individual groups. The addresses of these hosts can be described using ranges and other operations. Finally, each host can have custom variables, such as SSH and sudo passwords.

Playbooks are the core of Ansible and the basis for a really simple configuration management and multi-machine deployment system. Playbooks can declare configurations, but they can also orchestrate steps of any manually ordered process, even as different steps must bounce back and forth between sets of machines in particular orders. They can launch tasks synchronously or asynchronously.

Playbooks are expressed in YAML format which makes them very simple and readable. Each playbook is composed of one or more plays in a list. The goal of a play is to map a group of hosts to some well-defined roles, represented by things Ansible calls tasks. At a basic level, a task is nothing more than a call to an Ansible module.

By composing a playbook with multiple plays, it is possible to orchestrate multi-machine deployments, running certain steps on all machines in the web servers group, then certain steps on the database server group, then more commands back on the web servers group, etc. 
Playbooks have the interesting characteristic of being idempotent, they ensure the system is in the desired state, and only run tasks if needed.

