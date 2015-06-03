# Ansible 101

<br/>
![Linkedcare](images/linkedcare.png)
<br/>
Manuel Tiago Pereira

2015-05-27

---

## What is Ansible?
  - A configuration management tool with batteries included
  - Simplifies infrastructure provision, configuration and orchestration
  - Decentralized, agentless and push-based
  - Uses SSH for communication
  - Declarative language
  - Human-readable YAML files

---

## Why use Ansible?
  - Simple to use and fast to learn
  - Automates tasks in no time
  - Structured, reusable and VCS-manageable infrastructure descriptions
  - Mostly idempotent
  - Great replacement for bash as "infrastructure glue" for sysadmins and alike
  - 284 modules available and 2620 roles on https://galaxy.ansible.com

---

## Basic commands
There are 2 essencial commands
  - `ansible`
  - `ansible-playbook`

---

## Basic commands
### `ansible`
    $ ansible -i ../inventory -m setup -a 'filter=ansible_distribution' web1
    10.0.21.2 | success >> {
        "ansible_facts": {
            "ansible_distribution": "Ubuntu"
        },
        "changed": false
    }

---

## Basic commands
### `ansible-playbook`
    $ ansible-playbook -i ../inventory playbook.yml
    PLAY [Check filter module] ****************************************************

    GATHERING FACTS ***************************************************************
    ok: [10.0.21.2]

    TASK: [Get a couple of facts from the system] *********************************
    ok: [10.0.21.2] => (item=ansible_ssh_user) => {
        "item": "ansible_ssh_user",
        "var": {
            "ansible_ssh_user": "vagrant"
        }
    }

    [...]

    PLAY RECAP ********************************************************************
    10.0.21.2                  : ok=2    changed=0    unreachable=0    failed=0

---

## Basic commands
### Notable flags

  - `--check, -C`
  - `--syntax-check`
  - `--verbose, -v[vvv]`
  - `--limit, -l`
  - `--user, -u`
  - `--list-hosts`

---

## Inventories

  - Define your set of hosts
  - Organize hosts by groups
  - Define communication settings (IP address, username, etc)
  - Can be static or dynamic

---

## Inventories

    [web1]
    10.0.21.2 ansible_ssh_user=vagrant ansible_ssh_private_key_file=~/.vagrant.d/insecure_private_key

    [web2]
    10.0.21.3 ansible_ssh_user=vagrant ansible_ssh_private_key_file=~/.vagrant.d/insecure_private_key

    [webservers:children]
    web1
    web2

---

## Playbooks

  - "If Ansible modules are the tools in your workshop, playbooks are your design plans." @ http://docs.ansible.com/playbooks.html
  - An ordered plan of tasks to be executed on several sets of hosts
  - Composed of *Plays*

---

## Playbooks
### Plays

Plays describe a sequence of tasks to execute on a set of hosts and are defined by:

  - a `name`
  - `hosts` where they'll be applied
  - `remote_user` that'll connect to the hosts
  - `tasks` and `roles` that will be executed

---

## Playbooks
### Plays - `1/playbook.yml`

	- name: Check filter module
	  hosts: all
      remote_user: vagrant

	  tasks:
	    - name: Get a couple of facts from the host
	      debug:
	        var: "{{ item }}"
	      with_items:
	        - "ansible_ssh_user"
	        - "ansible_distribution"
	        - "ansible_distribution_release"

---

## Playbooks
### Tasks

  - The actual building blocks of our Plays
  - Tasks are calls to modules
  - Composed of a name, a module, it's parameters and additional options
  - You can write your own modules and effectively add new possible tasks
  - `ansible-doc --list` and `ansible-doc <module>` are your friends!

---

## Playbooks
### Tasks

**Handlers** are a special type of Tasks:

  - Triggered by other tasks, using `notify`
  - A handler only runs once, regardless of how many times it was triggered
  - Executed at the end of the Play

---

## Playbooks
### Tasks - `2/playbook.yml`

  - Deploys app on both webservers
  - Idempotency for source code changes
  - Every value is hardcoded on the Playbook
  - No way to reuse parts of the Play

---

## Playbooks
### Includes - `3/playbook.yml`

  - `include` enables reusing tasks, handlers and even whole playbooks

---

## Playbooks
### Variables and Facts

  - Adapt to different hosts or environments
  - Variables can be set on playbooks, included files and via command line
  - Facts are fetched automatically by Ansible from the host's characteristics
  - Facts can also be set dynamically, using `set_fact`

---

## Playbooks
### Variables and Facts - `4/playbook.yml`

  - Variables are automatically included from `group_vars/`
  - Hierarchy on groups defines variables priority
  - "Namespaces" on vars help us avoiding collisions and identifying associated tasks

---

## Playbooks
### Roles

  - Abstraction for modelling hosts
  - Enables reuse and distribution of automations
  - Use a specific directory structure to organize the elements we've seen so far
  - Default values for vars on `defaults/`
  - Internal vars on `vars/`

---

## Playbooks
### Roles - `5/playbook.yml`

  - 2 roles: `ruby` and `deploy`
  - Playbook is now much simpler
  - Now we can copy this roles and use them on other playbooks


