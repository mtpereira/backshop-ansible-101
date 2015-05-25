# Ansible 101

<br/>
![Linkedcare](images/linkedcare.png)

---

## What is Ansible?
- A configuration management tool with batteries included
- Simplifies infrastructure provision, configuration and orchestration
- Decentralized and agent-free
- Declarative language

---

## Why use Ansible?
  - Simple to use and fast to learn
  - Automates tasks in no time
  - Allows structured and reusable infrastructure description
  - Great replacement for bash as "infrastructure glue" for sysadmins and alike

---

## Basic commands
There are 2 essencial commands
  - `ansible`
  - `ansible-playbook`

---

## `ansible`
    $ ansible -i inventory -m setup -a 'filter=ansible_distribution' web1
    10.0.21.2 | success >> {
        "ansible_facts": {
            "ansible_distribution": "Ubuntu"
        },
        "changed": false
    }

---

## `ansible-playbook`
    $ ansible-playbook -i inventory 1.yml
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
<!-- .slide: data-background="#555555" -->
## A slide with a different background

