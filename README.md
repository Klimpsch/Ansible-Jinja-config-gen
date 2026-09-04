# Config generator with Ansible + Jinja

Automate device configuration, Reduce manual effort and improve consistency.

```
host_vars/<host>.yml  +  templates/base.j2   --> ansible-playbook render.yml -->   configs/<host>.cfg
```

## Description

Automate the configuration of network devices using Ansible + Jinja templates. 


## Installation
```bash
git clone https://github.com/Klimpsch/Ansible-Jinja-config-gen
cd Ansible-Jinja-config-gen
python -m venv venv
source venv/bin/activate      
pip install -r requirements.txt
```

## Run it

```bash
ansible-playbook render.yml
```

Inventory is picked up automatically via `ansible.cfg`.
`configs/R1.cfg`, `R2.cfg`, `SW1.cfg`, `SW2.cfg`.


## What each file does

| File | Role |
|------|------|
| `inventory.ini` | lists the devices (R1, R2, SW1, SW2) and groups them 
| `host_vars/<host>.yml` | per-device variables, auto-loaded by hostname 
| `templates/base.j2` | the Jinja template (almost identical) 
| `render.yml` | the playbook that renders each host 
| `ansible.cfg` | project settings (points at the inventory) 


The playbook's core is a single task:

```yaml
- name: Generate configs/{{ inventory_hostname }}.cfg
  ansible.builtin.template:
    src: templates/base.j2
    dest: "configs/{{ inventory_hostname }}.cfg"
```
